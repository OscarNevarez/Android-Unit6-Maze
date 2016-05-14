# Android-Unit6-Maze
package com.didactic.PrimDijkstraMaze;

import android.graphics.Canvas;
import android.graphics.Paint;
import android.os.Handler;
import android.service.wallpaper.WallpaperService;
import android.service.wallpaper.WallpaperService.Engine;
import android.util.Log;
import android.view.SurfaceHolder;
import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.LinkedList;
import java.util.Queue;

public class PrimDijkstraWallpaper
  extends WallpaperService
{
  private Paint black;
  private Paint blue;
  private ArrayList<Point> frontier;
  private Paint green;
  private final Handler mHandler = new Handler();
  private boolean mVisible;
  private boolean[][] maz;
  private boolean prim;
  private Paint red;
  private Paint white;
  
  public WallpaperService.Engine onCreateEngine()
  {
    return new PrimDijkstraEngine();
  }
  
  class Point
  {
    Integer c;
    Point parent;
    Integer r;
    
    public Point(int paramInt1, int paramInt2, Point paramPoint)
    {
      this.r = Integer.valueOf(paramInt1);
      this.c = Integer.valueOf(paramInt2);
      this.parent = paramPoint;
    }
    
    public boolean equals(Object paramObject)
    {
      if (this == paramObject) {
        return true;
      }
      if (!(paramObject instanceof Point)) {
        return false;
      }
      paramObject = (Point)paramObject;
      if (this.parent != null) {
        return (opposite().r == ((Point)paramObject).opposite().r) && (opposite().c == ((Point)paramObject).opposite().c);
      }
      return (this.r == ((Point)paramObject).r) && (this.c == ((Point)paramObject).c);
    }
    
    public Point opposite()
    {
      if (this.parent == null) {
        return null;
      }
      if (this.r.compareTo(this.parent.r) != 0) {
        return new Point(PrimDijkstraWallpaper.this, this.r.intValue() + this.r.compareTo(this.parent.r), this.c.intValue(), this);
      }
      if (this.c.compareTo(this.parent.c) != 0) {
        return new Point(PrimDijkstraWallpaper.this, this.r.intValue(), this.c.intValue() + this.c.compareTo(this.parent.c), this);
      }
      return null;
    }
  }
  
  public class PrimDijkstraEngine
    extends WallpaperService.Engine
  {
    private String[] ar;
    private ArrayList<Character> dpath;
    private PrimDijkstraWallpaper.Point dt = null;
    private PrimDijkstraWallpaper.Point finish;
    private int height;
    private PrimDijkstraWallpaper.Point lt = null;
    private final Runnable mDraw = new Runnable()
    {
      public void run()
      {
        PrimDijkstraWallpaper.PrimDijkstraEngine.this.update();
      }
    };
    private String mstr;
    private boolean[] opt;
    private Queue<Character> path;
    private PrimDijkstraWallpaper.Point start;
    private int width;
    private int x = 0;
    
    PrimDijkstraEngine()
    {
      super();
      PrimDijkstraWallpaper.this.white = new Paint();
      PrimDijkstraWallpaper.this.red = new Paint();
      PrimDijkstraWallpaper.this.blue = new Paint();
      PrimDijkstraWallpaper.this.green = new Paint();
      PrimDijkstraWallpaper.this.black = new Paint();
      PrimDijkstraWallpaper.this.white.setColor(-1);
      PrimDijkstraWallpaper.this.white.setAntiAlias(true);
      PrimDijkstraWallpaper.this.red.setColor(-65536);
      PrimDijkstraWallpaper.this.red.setAntiAlias(true);
      PrimDijkstraWallpaper.this.blue.setColor(-16776961);
      PrimDijkstraWallpaper.this.blue.setAntiAlias(true);
      PrimDijkstraWallpaper.this.green.setColor(-16711936);
      PrimDijkstraWallpaper.this.green.setAntiAlias(true);
      PrimDijkstraWallpaper.this.black.setColor(-16777216);
      PrimDijkstraWallpaper.this.black.setAntiAlias(true);
    }
    
    private void dijkstraInit()
    {
      PrimDijkstraWallpaper.this.prim = false;
      StringBuilder localStringBuilder = new StringBuilder();
      boolean[][] arrayOfBoolean = PrimDijkstraWallpaper.this.maz;
      int k = arrayOfBoolean.length;
      int i = 0;
      if (i >= k)
      {
        this.mstr = localStringBuilder.toString();
        switch ((int)(Math.random() * 4.0D))
        {
        }
      }
      for (;;)
      {
        this.path = new LinkedList();
        this.dpath = new ArrayList();
        this.dt = null;
        this.ar = new String[PrimDijkstraWallpaper.this.maz.length * PrimDijkstraWallpaper.this.maz[0].length];
        this.ar[(this.start.r.intValue() * PrimDijkstraWallpaper.this.maz[0].length + this.start.c.intValue())] = ((char)(this.start.r.intValue() * PrimDijkstraWallpaper.this.maz[0].length + this.start.c.intValue()));
        this.opt = new boolean[this.ar.length];
        return;
        boolean[] arrayOfBoolean1 = arrayOfBoolean[i];
        int m = arrayOfBoolean1.length;
        int j = 0;
        if (j >= m)
        {
          i += 1;
          break;
        }
        if (arrayOfBoolean1[j] != 0) {}
        for (char c = 't';; c = 'f')
        {
          localStringBuilder.append(c);
          j += 1;
          break;
        }
        do
        {
          i = (int)(Math.random() * PrimDijkstraWallpaper.this.maz.length / 4.0D + PrimDijkstraWallpaper.this.maz.length / 8);
          j = (int)(Math.random() * PrimDijkstraWallpaper.this.maz[0].length / 4.0D + PrimDijkstraWallpaper.this.maz[0].length / 8);
        } while (PrimDijkstraWallpaper.this.maz[i][j] == 0);
        Log.i("fml", "start");
        this.start = new PrimDijkstraWallpaper.Point(PrimDijkstraWallpaper.this, i, j, null);
        do
        {
          i = (int)(Math.random() * PrimDijkstraWallpaper.this.maz.length / 4.0D + PrimDijkstraWallpaper.this.maz.length * 5 / 8);
          j = (int)(Math.random() * PrimDijkstraWallpaper.this.maz[0].length / 4.0D + PrimDijkstraWallpaper.this.maz[0].length * 5 / 8);
        } while (PrimDijkstraWallpaper.this.maz[i][j] == 0);
        Log.i("fml", "finish");
        this.finish = new PrimDijkstraWallpaper.Point(PrimDijkstraWallpaper.this, i, j, null);
        continue;
        do
        {
          i = (int)(Math.random() * PrimDijkstraWallpaper.this.maz.length / 4.0D + PrimDijkstraWallpaper.this.maz.length * 5 / 8);
          j = (int)(Math.random() * PrimDijkstraWallpaper.this.maz[0].length / 4.0D + PrimDijkstraWallpaper.this.maz[0].length / 8);
        } while (PrimDijkstraWallpaper.this.maz[i][j] == 0);
        Log.i("fml", "start");
        this.start = new PrimDijkstraWallpaper.Point(PrimDijkstraWallpaper.this, i, j, null);
        do
        {
          i = (int)(Math.random() * PrimDijkstraWallpaper.this.maz.length / 4.0D + PrimDijkstraWallpaper.this.maz.length / 8);
          j = (int)(Math.random() * PrimDijkstraWallpaper.this.maz[0].length / 4.0D + PrimDijkstraWallpaper.this.maz[0].length * 5 / 8);
        } while (PrimDijkstraWallpaper.this.maz[i][j] == 0);
        Log.i("fml", "finish");
        this.finish = new PrimDijkstraWallpaper.Point(PrimDijkstraWallpaper.this, i, j, null);
        continue;
        do
        {
          i = (int)(Math.random() * PrimDijkstraWallpaper.this.maz.length / 4.0D + PrimDijkstraWallpaper.this.maz.length / 8);
          j = (int)(Math.random() * PrimDijkstraWallpaper.this.maz[0].length / 4.0D + PrimDijkstraWallpaper.this.maz[0].length * 5 / 8);
        } while (PrimDijkstraWallpaper.this.maz[i][j] == 0);
        Log.i("fml", "start");
        this.start = new PrimDijkstraWallpaper.Point(PrimDijkstraWallpaper.this, i, j, null);
        do
        {
          i = (int)(Math.random() * PrimDijkstraWallpaper.this.maz.length / 4.0D + PrimDijkstraWallpaper.this.maz.length * 5 / 8);
          j = (int)(Math.random() * PrimDijkstraWallpaper.this.maz[0].length / 4.0D + PrimDijkstraWallpaper.this.maz[0].length / 8);
        } while (PrimDijkstraWallpaper.this.maz[i][j] == 0);
        Log.i("fml", "finish");
        this.finish = new PrimDijkstraWallpaper.Point(PrimDijkstraWallpaper.this, i, j, null);
        continue;
        do
        {
          i = (int)(Math.random() * PrimDijkstraWallpaper.this.maz.length / 4.0D + PrimDijkstraWallpaper.this.maz.length * 5 / 8);
          j = (int)(Math.random() * PrimDijkstraWallpaper.this.maz[0].length / 4.0D + PrimDijkstraWallpaper.this.maz[0].length * 5 / 8);
        } while (PrimDijkstraWallpaper.this.maz[i][j] == 0);
        Log.i("fml", "start");
        this.start = new PrimDijkstraWallpaper.Point(PrimDijkstraWallpaper.this, i, j, null);
        do
        {
          i = (int)(Math.random() * PrimDijkstraWallpaper.this.maz.length / 4.0D + PrimDijkstraWallpaper.this.maz.length / 8);
          j = (int)(Math.random() * PrimDijkstraWallpaper.this.maz[0].length / 4.0D + PrimDijkstraWallpaper.this.maz[0].length / 8);
        } while (PrimDijkstraWallpaper.this.maz[i][j] == 0);
        Log.i("fml", "finish");
        this.finish = new PrimDijkstraWallpaper.Point(PrimDijkstraWallpaper.this, i, j, null);
      }
    }
    
    private void dijkstraStep()
    {
      char[] arrayOfChar;
      int j;
      int i;
      if (this.start.r.intValue() < this.finish.r.intValue())
      {
        this.x += 1;
        this.x %= this.ar.length;
        if ((this.ar[this.x] != null) && (this.opt[this.x] == 0))
        {
          this.opt[this.x] = true;
          this.dt = new PrimDijkstraWallpaper.Point(PrimDijkstraWallpaper.this, this.x / PrimDijkstraWallpaper.this.maz[0].length, this.x % PrimDijkstraWallpaper.this.maz[0].length, null);
          if ((this.x % PrimDijkstraWallpaper.this.maz[0].length != 0) && (this.ar[(this.x - 1)] == null) && (this.mstr.charAt(this.x - 1) != 'f')) {
            this.ar[(this.x - 1)] = (this.ar[this.x] + (char)(this.x - 1));
          }
          if ((this.x % PrimDijkstraWallpaper.this.maz[0].length != PrimDijkstraWallpaper.this.maz[0].length - 1) && (this.ar[(this.x + 1)] == null) && (this.mstr.charAt(this.x + 1) != 'f')) {
            this.ar[(this.x + 1)] = (this.ar[this.x] + (char)(this.x + 1));
          }
          if ((this.x / PrimDijkstraWallpaper.this.maz[0].length != 0) && (this.ar[(this.x - PrimDijkstraWallpaper.this.maz[0].length)] == null) && (this.mstr.charAt(this.x - PrimDijkstraWallpaper.this.maz[0].length) != 'f')) {
            this.ar[(this.x - PrimDijkstraWallpaper.this.maz[0].length)] = (this.ar[this.x] + (char)(this.x - PrimDijkstraWallpaper.this.maz[0].length));
          }
          if ((this.x / PrimDijkstraWallpaper.this.maz[0].length != PrimDijkstraWallpaper.this.maz.length - 1) && (this.ar[(this.x + PrimDijkstraWallpaper.this.maz[0].length)] == null) && (this.mstr.charAt(this.x + PrimDijkstraWallpaper.this.maz[0].length) != 'f')) {
            this.ar[(this.x + PrimDijkstraWallpaper.this.maz[0].length)] = (this.ar[this.x] + (char)(this.x + PrimDijkstraWallpaper.this.maz[0].length));
          }
        }
        if ((this.ar[(this.finish.r.intValue() * PrimDijkstraWallpaper.this.maz[0].length + this.finish.c.intValue())] != null) && (this.path.size() < 1))
        {
          arrayOfChar = this.ar[(this.finish.r.intValue() * PrimDijkstraWallpaper.this.maz[0].length + this.finish.c.intValue())].toCharArray();
          j = arrayOfChar.length;
          i = 0;
        }
      }
      for (;;)
      {
        if (i >= j)
        {
          return;
          this.x -= 1;
          if (this.x >= 0) {
            break;
          }
          this.x = (this.ar.length - 1);
          break;
        }
        char c = arrayOfChar[i];
        this.path.add(Character.valueOf(c));
        i += 1;
      }
    }
    
    private void drawThings(Canvas paramCanvas)
    {
      int i;
      int j;
      if (PrimDijkstraWallpaper.this.prim)
      {
        if ((this.lt != null) || (!PrimDijkstraWallpaper.this.prim)) {
          i = 0;
        }
        for (;;)
        {
          if (i >= PrimDijkstraWallpaper.this.maz.length)
          {
            return;
            primStep(paramCanvas);
            break;
          }
          j = 0;
          if (j < PrimDijkstraWallpaper.this.maz[0].length) {
            break label73;
          }
          i += 1;
        }
        label73:
        if ((PrimDijkstraWallpaper.this.maz[i][j] != 0) && (this.lt != null) && (i == this.lt.r.intValue()) && (j == this.lt.c.intValue()))
        {
          paramCanvas.drawRect(j * 10, i * 10, j * 10 + 10, i * 10 + 10, PrimDijkstraWallpaper.this.red);
          this.lt = null;
        }
        for (;;)
        {
          j += 1;
          break;
          if (PrimDijkstraWallpaper.this.maz[i][j] != 0) {
            paramCanvas.drawRect(j * 10, i * 10, j * 10 + 10, i * 10 + 10, PrimDijkstraWallpaper.this.white);
          } else {
            paramCanvas.drawRect(j * 10, i * 10, j * 10 + 10, i * 10 + 10, PrimDijkstraWallpaper.this.black);
          }
        }
      }
      if (this.ar[(this.finish.r.intValue() * PrimDijkstraWallpaper.this.maz[0].length + this.finish.c.intValue())] == null)
      {
        if ((this.dt != null) || (PrimDijkstraWallpaper.this.prim)) {
          i = 0;
        }
        for (;;)
        {
          if (i >= PrimDijkstraWallpaper.this.maz.length)
          {
            paramCanvas.drawRect(this.finish.c.intValue() * 10, this.finish.r.intValue() * 10, this.finish.c.intValue() * 10 + 10, this.finish.r.intValue() * 10 + 10, PrimDijkstraWallpaper.this.green);
            paramCanvas.drawRect(this.start.c.intValue() * 10, this.start.r.intValue() * 10, this.start.c.intValue() * 10 + 10, this.start.r.intValue() * 10 + 10, PrimDijkstraWallpaper.this.green);
            return;
            dijkstraStep();
            break;
          }
          j = 0;
          if (j < PrimDijkstraWallpaper.this.maz[0].length) {
            break label513;
          }
          i += 1;
        }
        label513:
        if ((PrimDijkstraWallpaper.this.maz[i][j] != 0) && (this.dt != null) && (i == this.dt.r.intValue()) && (j == this.dt.c.intValue())) {
          if (this.ar[(this.finish.r.intValue() * PrimDijkstraWallpaper.this.maz[0].length + this.finish.c.intValue())] == null)
          {
            paramCanvas.drawRect(this.dt.c.intValue() * 10, this.dt.r.intValue() * 10, this.dt.c.intValue() * 10 + 10, this.dt.r.intValue() * 10 + 10, PrimDijkstraWallpaper.this.blue);
            label675:
            this.dt = null;
          }
        }
        for (;;)
        {
          j += 1;
          break;
          paramCanvas.drawRect(this.dt.c.intValue() * 10, this.dt.r.intValue() * 10, this.dt.c.intValue() * 10 + 10, this.dt.r.intValue() * 10 + 10, PrimDijkstraWallpaper.this.white);
          break label675;
          if (PrimDijkstraWallpaper.this.maz[i][j] != 0) {
            paramCanvas.drawRect(j * 10, i * 10, j * 10 + 10, i * 10 + 10, PrimDijkstraWallpaper.this.white);
          }
        }
      }
      Log.i("fml", this.path.size());
      Object localObject = (Character)this.path.poll();
      if (localObject != null)
      {
        this.dpath.add(localObject);
        i = 0;
        if (i >= PrimDijkstraWallpaper.this.maz.length) {
          localObject = this.dpath.iterator();
        }
        for (;;)
        {
          if (!((Iterator)localObject).hasNext())
          {
            paramCanvas.drawRect(this.finish.c.intValue() * 10, this.finish.r.intValue() * 10, this.finish.c.intValue() * 10 + 10, this.finish.r.intValue() * 10 + 10, PrimDijkstraWallpaper.this.green);
            paramCanvas.drawRect(this.start.c.intValue() * 10, this.start.r.intValue() * 10, this.start.c.intValue() * 10 + 10, this.start.r.intValue() * 10 + 10, PrimDijkstraWallpaper.this.green);
            return;
            j = 0;
            for (;;)
            {
              if (j >= PrimDijkstraWallpaper.this.maz[0].length)
              {
                i += 1;
                break;
              }
              if (PrimDijkstraWallpaper.this.maz[i][j] != 0) {
                paramCanvas.drawRect(j * 10, i * 10, j * 10 + 10, i * 10 + 10, PrimDijkstraWallpaper.this.white);
              }
              j += 1;
            }
          }
          Character localCharacter = (Character)((Iterator)localObject).next();
          paramCanvas.drawRect(localCharacter.charValue() % PrimDijkstraWallpaper.this.maz[0].length * 10, localCharacter.charValue() / PrimDijkstraWallpaper.this.maz[0].length * 10, localCharacter.charValue() % PrimDijkstraWallpaper.this.maz[0].length * 10 + 10, localCharacter.charValue() / PrimDijkstraWallpaper.this.maz[0].length * 10 + 10, PrimDijkstraWallpaper.this.blue);
        }
      }
      primInit();
    }
    
    private void primInit()
    {
      PrimDijkstraWallpaper.this.prim = true;
      Object localObject = PrimDijkstraWallpaper.this;
      int i = this.height / 10;
      int j = this.width / 10;
      ((PrimDijkstraWallpaper)localObject).maz = ((boolean[][])Array.newInstance(Boolean.TYPE, new int[] { i, j }));
      localObject = new PrimDijkstraWallpaper.Point(PrimDijkstraWallpaper.this, (int)(Math.random() * PrimDijkstraWallpaper.this.maz.length / 2.0D + PrimDijkstraWallpaper.this.maz.length / 4), (int)(Math.random() * PrimDijkstraWallpaper.this.maz[0].length / 2.0D + PrimDijkstraWallpaper.this.maz[0].length / 4), null);
      Log.i("gml", this.height + " " + this.width);
      PrimDijkstraWallpaper.this.maz[localObject.r.intValue()][localObject.c.intValue()] = 1;
      PrimDijkstraWallpaper.this.frontier = new ArrayList();
      i = -1;
      for (;;)
      {
        if (i > 1)
        {
          this.lt = null;
          return;
        }
        j = -1;
        if (j <= 1) {
          break;
        }
        i += 1;
      }
      if (((i == 0) && (j == 0)) || ((i != 0) && (j != 0))) {}
      for (;;)
      {
        j += 1;
        break;
        try
        {
          int k = PrimDijkstraWallpaper.this.maz[(localObject.r.intValue() + i)][(localObject.c.intValue() + j)];
          if (k == 0) {
            PrimDijkstraWallpaper.this.frontier.add(new PrimDijkstraWallpaper.Point(PrimDijkstraWallpaper.this, ((PrimDijkstraWallpaper.Point)localObject).r.intValue() + i, ((PrimDijkstraWallpaper.Point)localObject).c.intValue() + j, (PrimDijkstraWallpaper.Point)localObject));
          }
        }
        catch (Exception localException) {}
      }
    }
    
    private void primStep(Canvas paramCanvas)
    {
      localPoint = (PrimDijkstraWallpaper.Point)PrimDijkstraWallpaper.this.frontier.remove((int)(Math.random() * PrimDijkstraWallpaper.this.frontier.size()));
      paramCanvas = localPoint.opposite();
      try
      {
        if ((PrimDijkstraWallpaper.this.maz[localPoint.r.intValue()][localPoint.c.intValue()] == 0) && (PrimDijkstraWallpaper.this.maz[paramCanvas.r.intValue()][paramCanvas.c.intValue()] == 0))
        {
          PrimDijkstraWallpaper.this.maz[localPoint.r.intValue()][localPoint.c.intValue()] = 1;
          PrimDijkstraWallpaper.this.maz[paramCanvas.r.intValue()][paramCanvas.c.intValue()] = 1;
          this.lt = paramCanvas;
          i = -1;
          if (i <= 1) {
            break label171;
          }
        }
      }
      catch (Exception paramCanvas)
      {
        for (;;)
        {
          try
          {
            int i;
            int j;
            int k = PrimDijkstraWallpaper.this.maz[(paramCanvas.r.intValue() + i)][(paramCanvas.c.intValue() + j)];
            if (k != 0) {
              continue;
            }
            localPoint = new PrimDijkstraWallpaper.Point(PrimDijkstraWallpaper.this, paramCanvas.r.intValue() + i, paramCanvas.c.intValue() + j, paramCanvas);
            if (PrimDijkstraWallpaper.this.frontier.contains(localPoint)) {
              continue;
            }
            PrimDijkstraWallpaper.this.frontier.add(localPoint);
          }
          catch (Exception localException) {}
          paramCanvas = paramCanvas;
        }
      }
      if (PrimDijkstraWallpaper.this.frontier.isEmpty()) {
        dijkstraInit();
      }
      return;
      label171:
      j = -1;
      for (;;)
      {
        if (j > 1)
        {
          i += 1;
          break;
        }
        if (((i != 0) || (j != 0)) && ((i == 0) || (j == 0))) {
          break label208;
        }
        j += 1;
      }
    }
    
    private void update()
    {
      SurfaceHolder localSurfaceHolder = getSurfaceHolder();
      Object localObject1 = null;
      try
      {
        Canvas localCanvas = localSurfaceHolder.lockCanvas();
        if (localCanvas != null)
        {
          localObject1 = localCanvas;
          drawThings(localCanvas);
        }
        if (localCanvas != null) {
          localSurfaceHolder.unlockCanvasAndPost(localCanvas);
        }
        PrimDijkstraWallpaper.this.mHandler.removeCallbacks(this.mDraw);
        if (PrimDijkstraWallpaper.this.mVisible) {
          PrimDijkstraWallpaper.this.mHandler.postDelayed(this.mDraw, 100L);
        }
        return;
      }
      finally
      {
        if (localObject1 != null) {
          localSurfaceHolder.unlockCanvasAndPost((Canvas)localObject1);
        }
      }
    }
    
    public void onCreate(SurfaceHolder paramSurfaceHolder)
    {
      super.onCreate(paramSurfaceHolder);
    }
    
    public void onDestroy()
    {
      super.onDestroy();
      PrimDijkstraWallpaper.this.mHandler.removeCallbacks(this.mDraw);
    }
    
    public void onSurfaceChanged(SurfaceHolder paramSurfaceHolder, int paramInt1, int paramInt2, int paramInt3)
    {
      super.onSurfaceChanged(paramSurfaceHolder, paramInt1, paramInt2, paramInt3);
      setSurfaceSize(paramInt2, paramInt3);
      update();
    }
    
    public void onSurfaceCreated(SurfaceHolder paramSurfaceHolder)
    {
      super.onSurfaceCreated(paramSurfaceHolder);
    }
    
    public void onSurfaceDestroyed(SurfaceHolder paramSurfaceHolder)
    {
      super.onSurfaceDestroyed(paramSurfaceHolder);
      PrimDijkstraWallpaper.this.mHandler.removeCallbacks(this.mDraw);
    }
    
    public void onVisibilityChanged(boolean paramBoolean)
    {
      PrimDijkstraWallpaper.this.mVisible = paramBoolean;
      if (paramBoolean)
      {
        update();
        return;
      }
      PrimDijkstraWallpaper.this.mHandler.removeCallbacks(this.mDraw);
    }
    
    public void setSurfaceSize(int paramInt1, int paramInt2)
    {
      this.width = paramInt1;
      this.height = paramInt2;
      primInit();
    }
  }
}
