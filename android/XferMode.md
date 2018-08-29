## XferMode

XferMode는 사용자가 View를 그릴 때 커스텀으로 핸들링할 때 쓸 수 있는 모드이따. 아래와 같이 Src, Dst라는 패스가 있을 때, 각각의 모드 별로 하는 동작을 사진으로 정리해뒀다.

![XferMode정리](http://upload-images.jianshu.io/upload_images/2220902-c3b12184d8def8a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


아래는 라운드 처리된 이미지를 그리는 예시다.

```
//페인트 선언
//PorterDuff.Mode.DST_OUT는 겹치는 부분을 제외한 부분을 그린다.
 Paint maskPaint = new Paint();
       maskPaint.setAntiAlias(true);
       maskPaint.setColor(Color.BLACK);
       maskPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.DST_OUT));

```

```
//패스 사각형과 라운드 처리된 Rect 사이의 애들만 채움
//Path.FillType.EVEN_ODD는 서로 다른 패스의 사이 값만 채운다.

Path maskPath = new Path();
     maskPath.setFillType(Path.FillType.EVEN_ODD);
     maskPath.addRoundRect(new RectF(0, 0, width, height), roundR, Path.Direction.CW);
     maskPath.addRect(0, 0, width, height, Path.Direction.CW);
```

```
saveCount = canvas.saveLayer(0, 0, width, height, null, Canvas.ALL_SAVE_FLAG);
//이렇게 그리기 전에 레이어만 저장해뒀다가

super.draw(canvas);
//로 일단 이미지를 그린다. (네모네모 이미지)

canvas.drawPath(maskPath, maskPaint);
//라운드 처리된 모퉁이만 그려진 Path를 canvas에 그려진 값과 PorterDuff.Mode.DST_OUT 처리를 해준다. 

canvas.restoreToCount(saveCount);
//이전에 저장해둔 count로 되돌리게되면, Paint는 그려지고 그 사이값은 투명으로 처리되게 된다.
```


