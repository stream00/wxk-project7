# wxk-project7
```java
import controlP5.*;
import peasy.*;
PeasyCam cam;
ControlP5 bar;
int sz=2,bh=2,bj=3,ds=5,R=255,G=255,B=255;

ArrayList<KochLine> lines;
float r = 180;

void setup() {
  size(500,500);
  smooth(8);
  cam = new PeasyCam(this, 10);
  UI();
}

void draw() {

  background(0);
 UIShow();
  lines = new ArrayList<KochLine>();
  PVector a = new PVector(100,160);
  PVector b = new PVector(width-100, 160);
  PVector c = new PVector(width/2, width*cos(radians(30)));
  
  lines.add(new KochLine(a,b,r));
  lines.add(new KochLine(b,c,r));
  lines.add(new KochLine(c,a,r));

  for (int i = 0; i <ds; i++) {    
    generate();
  }  
  for (KochLine l : lines) {
    l.display();
  }
  r++;
}

void generate() {
  ArrayList next = new ArrayList<KochLine>();
  for (KochLine l : lines) {
    PVector a = l.kochA();
    PVector b = l.kochB();
    PVector c = l.kochC();
    PVector d = l.kochD();
    PVector e = l.kochE();

    next.add(new KochLine(a, b, r));
    next.add(new KochLine(b, c, r));
    next.add(new KochLine(c, d, r));
    next.add(new KochLine(d, e, r));
  }
  lines = next;
}



class KochLine {
  PVector start, end;
  float r;
  
  KochLine(PVector a, PVector b, float r_) {
    start = a.get();
    end = b.get();
    r = r_;

  }
  
  void display() {
    stroke(R,G,B);
    line(start.x, start.y, end.x, end.y);
  }
  
  PVector kochA() {
    return start.get();
  }
  
  PVector kochB() {
    PVector v = PVector.sub(end, start);
    v.div(bj);
    v.add(start);
    return v;
  }

  PVector kochC() {
    PVector a = start.get();

    PVector v = PVector.sub(end, start);
    v.div(bh);
    a.add(v);

    v.rotate(-radians(r));
    a.add(v);

    return a;
  }

  PVector kochD() {
    PVector v = PVector.sub(end, start);
    v.mult(sz/3.0);
    v.add(start);
    return v;
  }
  
  PVector kochE() {
    return end.get();
  }  
}



int canvasLeftCornerX = 30;
int canvasLeftCornerY = 60;



void s1() {
  size(500, 500);
 cam = new PeasyCam(this,500);
  UI();
}

void d1() {
  background(51);

  UIShow();
}



void UI() {
  bar = new ControlP5(this, createFont("微软雅黑", 14));
  int barSize = 100;
  int barHeight = 10;
  int barInterval = barHeight + 10;
  bar.addSlider("ds", 1, 7,5, canvasLeftCornerX, canvasLeftCornerY, barSize, barHeight).setLabel("代数");
  bar.addSlider("R", 0, 255, 255, canvasLeftCornerX, canvasLeftCornerY+barInterval, barSize, barHeight).setLabel("颜色R");
  bar.addSlider("G", 0, 255, 255, canvasLeftCornerX, canvasLeftCornerY+barInterval*3, barSize, barHeight).setLabel("颜色G");
  bar.addSlider("B", 0, 255, 255, canvasLeftCornerX, canvasLeftCornerY+barInterval*4, barSize, barHeight).setLabel("颜色B");
  bar.addSlider("bj", 1, 50, 3, canvasLeftCornerX, canvasLeftCornerY+barInterval*5, barSize, barHeight).setLabel("间隔边距");
  bar.addSlider("bh", 1, 10, 2,canvasLeftCornerX, canvasLeftCornerY+barInterval*6, barSize, barHeight).setLabel("变化程度");
    bar.addSlider("sz", 1, 7, 2,canvasLeftCornerX, canvasLeftCornerY+barInterval*7, barSize, barHeight).setLabel("线的数量");
  bar.setAutoDraw(false);
}



void UIShow() {
  cam.beginHUD();
 
  bar.draw();
  cam.endHUD();

}


