---
layout: post
title: 使用Quartz 2D实现径向渐变
date: '2015-11-19 06:58:05'
tags:
- ios
- quartz-2d
- uibezierpath
---



由于项目原因，需要实现一个雷达图，图上的线条颜色需要从内向外渐变，外圈上的点的颜色随当前部分所处区域不同而显示不同颜色。成果图如下，还未实现将折线变成曲线：

![结果](http://upload-images.jianshu.io/upload_images/1121432-fa8222dfafa0c82e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

线条和点都是使用`UIBezierPath`绘制,对于普通的绘图`UIBezierPath`就足够了，而且比较易于理解,绘图都在layer层完成。

### 轴向渐变与径向渐变

#### 轴向渐变(Axial,也称为线性渐变)

轴向渐变就是从一个点到另一个点的轴线渐变，所有位于垂直于轴线的某条线上的点都具有相同的值，在iOS中实现起来也是非常的简单。只需要一个在`CALayer`上封装好的`CAGradientLayer`就可以了，`CAGradientLayer`已经为我们封装好了渐变的方法，我们只需要指定需要的颜色，每种颜色的位置点，之后的渐变就交给系统处理。但是目前提供的字段比较少，开发者能自定义的也比较少。目前的`CAGradientLayer`虽然提供了`type`字段，但是坑爹的只支持轴向渐变，不支持径向渐变

```
/* The kind of gradient that will be drawn. Currently the only allowed
 * value is `axial' (the default value). */

@property(copy) NSString *type;

```

`CAGradientLayer`实际上是给我们提供了一个渐变的背景，如果要实现线条等等的渐变，还需要一个`CAShapeLayer`。`CAShapeLayer`上包含路径，将其设为mask加到`CAGradientLayer`，相当于在`CAGradientLayer`上抠出这个路径，大功告成。

我需要径向渐变，怎么办？？看来只能寻求更底层的方案了，毕竟封装的功能太少。就让Quartz 2D来拯救世界吧。

#### 径向渐变(Radial)

轴向渐变是从点到点，径向渐变就是圆到圆的辐射（圆也可以变成点）。由于系统分装的两个layer无法直接来实现径向渐变，所以就只能曲线救国了。在Quartz中的为`CGShadingRef`和`CGGradientRef`（其实我们只需要`CGGradientRef`就够了^^）。

##### CGGradientRef

`CGGradientRef`对象是一个渐变的抽象定义，它定义了指定颜色的位置，但是未指定形状，所以理论上我们想画成什么样子就可以画成什么样子。官方定义如下：

```
/* A CGGradient defines a transition between colors. The transition is
   defined over a range from 0 to 1 inclusive. A gradient specifies a color
   at location 0, one at location 1, and possibly additional colors assigned
   to locations between 0 and 1.

   A CGGradient has a color space. When a gradient is created, all colors
   specified are converted to that color space, and interpolation of colors
   occurs using the components of that color space. See the documentation of
   each creation function for more details. */

```

实现步骤也很简单，只有几步，代码也不多，直接上代码

```
    // 1.使用RGB模式的颜色空间
    CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceRGB();
    // 2.颜色空间，如果使用了RGB颜色空间则4个数字一组表示一个颜色，下面的数组表示4个颜色
    CGFloat colors[] = {1,0,0,1, 1,1,0,1, 0,1,0,1, 0,0,1,1};
    // 3.locations代表4个颜色的分布区域（0~1），如果需要均匀分布只需要传入NULL
    CGFloat locations[]={0.125,0.375,0.625,0.875};
    // 4. 创建CGGradient对象
    CGGradientRef gradient = CGGradientCreateWithColorComponents(colorSpace, colors, locations, 4);
    // 5. 绘制
    CGContextDrawRadialGradient (context, gradient, self.position,
                                 0, self.position, 140,
                                 kCGGradientDrawsAfterEndLocation);
    // 6. 需要释放对象
    CGColorSpaceRelease(colorSpace);
    CGGradientRelease(gradient);
```

其中绘制部分参数参见官方文档说明

```
/* Fill the current clipping region of `context' with a radial gradient
   between two circles defined by the center point and radius of each
   circle. The location 0 of `gradient' corresponds to a circle centered at
   `startCenter' with radius `startRadius'; the location 1 of `gradient'
   corresponds to a circle centered at `endCenter' with radius `endRadius';
   colors are linearly interpolated between these two circles based on the
   values of the gradient's locations. The option flags control whether the
   gradient is drawn before the start circle or after the end circle. */

CG_EXTERN void CGContextDrawRadialGradient(CGContextRef __nullable c,
    CGGradientRef __nullable gradient, CGPoint startCenter, CGFloat startRadius,
    CGPoint endCenter, CGFloat endRadius, CGGradientDrawingOptions options)
    CG_AVAILABLE_STARTING(__MAC_10_5, __IPHONE_2_0);

```

显示效果如下图

![显示效果](http://upload-images.jianshu.io/upload_images/1121432-8cba1e7feb3bac12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好像离成功越来越近了呢。

直接使用`CAShapeLayer`和`CAGradientLayer`创建线条的渐变我们已经会了，那就是同样的道理了。既然`CAGradientLayer`只是提供了一个颜色背景，那我们就自定义一个自己的`GradientLayer`，把轴向渐变绘制到这个layer上不就行咯。

那我们就继承一个`CALayer`（继承`CAGradientLayer`也是一样的，只是没有必要），在上面绘制一下，直接上核心代码（其实跟上面代码一样,在.m里重写`drawInContext`方法：

```
- (void)drawInContext:(CGContextRef)ctx {
    UIGraphicsPushContext(ctx);
    CGContextRef context = UIGraphicsGetCurrentContext();
    
    // 1.使用RGB模式的颜色空间
    CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceRGB();
    // 2.颜色空间，如果使用了RGB颜色空间则4个数字一组表示一个颜色，下面的数组表示4个颜色
    CGFloat colors[] = {1,0,0,1, 1,1,0,1, 0,1,0,1, 0,0,1,1};
    // 3.locations代表4个颜色的分布区域（0~1），如果需要均匀分布只需要传入NULL
    CGFloat locations[]={0.125,0.375,0.625,0.875};
    // 4. 创建CGGradient对象
    CGGradientRef gradient = CGGradientCreateWithColorComponents(colorSpace, colors, locations, 4);
    // 5. 绘制
    CGContextDrawRadialGradient (context, gradient, self.position,
                                 0, self.position, 140,
                                 kCGGradientDrawsAfterEndLocation);
    // 6. 需要释放对象
    CGColorSpaceRelease(colorSpace);
    CGGradientRelease(gradient);
    
    CGContextSaveGState(context);
    CGContextRestoreGState(context);
    UIGraphicsPopContext();
}

```
见证奇迹的时候就要到了！哦，等等，我们还需要一个ShapeLayer。那我们就创建一个`CAShapeLayer`对象，将线条的path加到shapeLayer上。上完代码（其实就跟使用`CAShapeLayer`和`CAGradientLayer`创建轴向渐变一模一样，只是将gradientLayer设置颜色和位置放到了类里面），让我们重新来见证奇迹吧!

```
    self.pathLayer.frame = self.bounds;
    self.pathLayer.path = line.CGPath;
    self.pathLayer.strokeColor = [[UIColor whiteColor] CGColor];
    self.pathLayer.fillColor = nil;
    self.pathLayer.lineWidth = 2.5;
    [self.pathLayer setEdgeAntialiasingMask:kCALayerLeftEdge | kCALayerRightEdge | kCALayerBottomEdge | kCALayerTopEdge];

    [self.gradientLayer setFrame:self.bounds];
    [self.gradientLayer setMask:self.pathLayer];
    [self addSublayer:self.gradientLayer];

```

成功如图：

![结果](http://upload-images.jianshu.io/upload_images/1121432-fa8222dfafa0c82e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其实也挺简单的，这样画一遍发现轴向渐变也是这样实现的，而`CAGradientLayer`只是在`CALayer`上做了一次封装，但是还未将径向渐变封装进去，如果感兴趣也可以仿照官方的样子封装一个带有径向渐变和轴向渐变的layer，使用type作区分。
