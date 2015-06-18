title: 为UI编程“提速”
speaker: dzpqzb
url: https://github.com/ksky521/nodePPT
transition: cards
files: /js/demo.js,/css/demo.css

[slide]

# 为UI编程“提速”
##一些UI层编码的小建议
## 演讲者：dzpqzb

[slide]

<img src="http://www.forkosh.com/mathtex.cgi?\large\color{yellow}\log{2^N}">

[slide]
## BLocks

![图片](/QQ20141126-1.png "图片")

```
 CGFloat offsetY = IS_IPHONE5 ? 80 : 50;
    CGFloat textOffsetY = CGRectGetMaxY(_urlImageView.frame) + offsetY;
    typedef UILabel*  (^GetLabel)(UIView* aView);
    void (^SetFameWithLabel)(CGFloat x, UIView* aView, GetLabel block) = ^(CGFloat x, UIView* aView, GetLabel block) {
        UILabel* label = block(aView);
        NSString* str = label.text;
        CGSize size = [str sizeWithFont:label.font];
        aView.frame = CGRectMake(x,
                                 textOffsetY,
                                 size.width, CGRectGetHeight(_checkButton.frame));
    };
    GetLabel lableBlock =  ^(UIView* aview) {
        return (UILabel*)aview;
    };
    GetLabel buttonBlock  =  ^(UIView* aview){
        UIButton* button = (UIButton*) aview;
        return button.titleLabel;
    };
    _checkButton.frame = CGRectMake(15, textOffsetY, 30, 30);
    SetFameWithLabel(CGRectGetMaxX(_checkButton.frame), _headLabel, lableBlock );
    SetFameWithLabel(CGRectGetMaxX(_headLabel.frame), _serviceContractButton, buttonBlock);
    SetFameWithLabel(CGRectGetMaxX(_serviceContractButton.frame), _tailLabel, lableBlock);
    SetFameWithLabel(CGRectGetMaxX(_tailLabel.frame), _userageButton, buttonBlock);
```

[slide]

##宏定义

----
<div class="columns-2">
    <pre><code class="Objective-C">
#define DEFINE_PROPERTY(mnmKind, type , name) @property (nonatomic, mnmKind)  type  name
#define DEFINE_PROPERTY_STRONG(type, name) DEFINE_PROPERTY(strong, type, name)
#define DEFINE_PROPERTY_STRONG_UILabel(name) DEFINE_PROPERTY_STRONG(UILabel*, name)

#define INIT_SUBVIEW(sView, class, name) name = [[class alloc] init]; [sView addSubview:name];
#define INIT_SUBVIEW_UILabel(sView, name) INIT_SUBVIEW(sView, UILabel, name)
#define INIT_SELF_SUBVIEW_UILabel(name) INIT_SUBVIEW_UILabel(self, name)

    </code></pre>

    <pre><code class="Objective-C"> 
     @interface LTFeedHeadView : UIView
    DEFINE_PROPERTY_STRONG(LTGrowLabel*,titleLabel);
    DEFINE_PROPERTY_STRONG(LTLikeButton*, commentButton);
    DEFINE_PROPERTY_STRONG(LTLikeButton*, lookedCountButton);
    DEFINE_PROPERTY_STRONG_UILabel(clubNickLabel);
    @end

- (instancetype) initWithFrame:(CGRect)framea {
....
    INIT_SELF_SUBVIEW(LTGrowLabel, _titleLabel);
    INIT_SELF_SUBVIEW_UILabel(_clubNickLabel);
    INIT_SELF_SUBVIEW(LTLikeButton, _lookedCountButton);
    INIT_SELF_SUBVIEW(LTLikeButton, _commentButton);

....
}

    </code></pre>
</div>

[slide]
函数

```
- (void) layoutSubViews
{
    [super layoutSubviews];
    CGRect contentRect = CGRectCenterSubSize(self.bounds, CGSizeMake(_xSpace*2, _ySpace*2));
    
    CGRect bottomRect;
    CGRect titleRect;
    CGRectDivide(contentRect, &bottomRect, &titleRect, _bottomHeight, CGRectMaxYEdge);
    titleRect.size.height -= _ySpace;
    
    _titleLabel.frame = titleRect;
    
    CGRect lookRect;
    CGRect commentRect;
    CGRect clubRect;
    .....
    _commentButton.frame = commentRect;
    _lookedCountButton.frame = lookRect;
    _clubNickLabel.frame = clubRect;
}
```
[slide  style="background-image:url('/http://www.forkosh.com/mathtex.cgi?\large\color{yellow}\log{2^N}')"]

##使用高阶抽象来提高编码效率，节省时间，以合适的方式“偷懒” 
{:&.flexbox.vleft}

-----
*常用方式
        1. 基于流程的抽象 {:&.moveIn}
        2. 基于功能描述抽象
-------


[slide]
##编码流程
![编码流程](/coding_process.jpg "编码流程")

>针对固定的流程可以通过一些类似代码生成的方式来，减少开发工作，提高代码质量

>人是流程中最容易出错的环节
[slide]
##功能抽象
![coding](/adstract_level.jpg "抽象层级")
>结合业务找到合适的抽象层级，以恰当的粒度构建DSL
[slide]
##“懒”是第一生产力  

##THANKS

[slide]
参考资料:

1. https://github.com/yishuiliunian/DZProgrameDefines
2. https://github.com/yishuiliunian/DZGeometryTools


