---
layout: post
title: "Multiple Rounded Corners With UIBezierPath"
date: 2013-09-05 17:23
comments: true
categories: iOS
---

Sometime ago I ran into a problem where I wanted to round one side of a UIView but leave the other size square. I had a good search around the internet and could not find a solution that was not crazy complicated than it should have been. Really all I wanted to do was round more than one corner but not all corners. 

##The options I thought I had was 

- UIRectCornerTopLeft
- UIRectCornerTopRight
- UIRectCornerBottomLeft
- UIRectCornerBottomRight
- UIRectCornerAllCorners

But what if I wanted UIRectCornerTopLeft & UIRectCornerTopRight To me there was no way I could only select one of these options. I did try the following, Which results in a compiler error. 



``` objective-c Incorrect Code Block
  [UIBezierPath bezierPathWithRoundedRect:self.view.bounds
                          byRoundingCorners:UIRectCornerTopRight | UIRectCornerTopLeft, UIRectCornerBottomRight
                               cornerRadii:CGSizeMake(10.0,10.0)];
```

So I went off and tried crazy custom UIBezierPath to try and draw my own and not using bezierPathWithRoundedRect. After much failure I randomly tried the Wonderful '|' character knowing sometimes I can chain commands using this character. 

## Well Look at that

It worked! I had no ideas you could use '|' to add more than one option which could be a valuable piece of information in the future when also using methods that allow me to pass in enum options. 


## Some examples

{% img http://scottr.me/images/postImages/UIBiezerPathImage.png  250 350 UIBiezerPathImage %}



So View 1 is set like the following. 

``` objective-c View 1 Code Block
    //set view 1
    -(UIBezierPath *)roundTopLeftOfView:(UIView *)view{
    // Create the path (with only the top-left corner rounded)
    return [UIBezierPath bezierPathWithRoundedRect:view.bounds
                                                   byRoundingCorners:UIRectCornerTopLeft
                                                         cornerRadii:CGSizeMake(10.0, 10.0)];
	}

    CAShapeLayer *maskLayerView1 = [CAShapeLayer layer];
    maskLayerView1.frame = _view1.bounds;
    maskLayerView1.path = [self roundTopLeftOfView:_view1].CGPath;
    _view1.layer.mask = maskLayerView1;
```

Some different methods you could call are

``` objective-c View 1 Code Block
**Round just the top right of a view**

	    return  [UIBezierPath bezierPathWithRoundedRect:view.bounds
	                                                   byRoundingCorners:UIRectCornerTopRight
	                                                         cornerRadii:CGSizeMake(10.0, 10.0)];
```

``` objective-c View 1 Code Block
**Round just the whole top of a view**

    return  [UIBezierPath bezierPathWithRoundedRect:view.bounds
                                  byRoundingCorners:UIRectCornerTopRight | UIRectCornerTopLeft
                                        cornerRadii:CGSizeMake(10.0, 10.0)];
    
```

**Round just the top and bottom right of a view**
``` objective-c View 1 Code Block
    return  [UIBezierPath bezierPathWithRoundedRect:view.bounds
                                  byRoundingCorners:UIRectCornerTopRight | UIRectCornerTopLeft | UIRectCornerBottomRight
                                        cornerRadii:CGSizeMake(10.0, 10.0)]; 

```

**Round everything!**
``` objective-c View 1 Code Block

    return  [UIBezierPath bezierPathWithRoundedRect:view.bounds
                                  byRoundingCorners:UIRectCornerAllCorners
                                        cornerRadii:CGSizeMake(10.0, 10.0)];
```

So there we go a very simple solution to a problem that took me way to long to solve, maybe it might help someone else somewhere. 


Thanks for reading, 
You can follow me on twitter at [@scottmkroberts](https://twitter.com/scottmkroberts)  or App.Net [@scottr](https://alpha.app.net/scottr)                                       
    


