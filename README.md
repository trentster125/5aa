# 5aa

import processing.sound.*;

void stupidshit(){
      SoundFile file;
      file = new SoundFile(this, "sample.mp3");
 
}
class Ball {
  float initialX, initialY;
  float x, y;
  float vx, vy;
  float size = 10;
  color c;
  SoundFile file;

  Ball(float x, float y, color c) {
    this.x = initialX = x;
    this.y = initialY = y;
    this.c = c;
    vx = 2+random(1);
    vy = random(1);
  }

  void draw() {
    noStroke();
    fill(c);
    ellipse(x, y, size*2, size*2);
  }

  void reset() {
    x = initialX;
    y = initialY;
    vx = 2+random(1);
    if (random(1) < 0.5) {
      vx = -vx;
    }
    vy = random(2) - 1;
  }

  void move() {
    x += vx;
    y += vy;
    if (x < size || x > width-size) {
      vx = -vx;
    }
    if (y < size || y > height-size) {
      vy = -vy;
    }
    if (x-size < leftPaddle.x + leftPaddle.w) {
      if (y > leftPaddle.y &&
          y < leftPaddle.y + leftPaddle.h) {
        vx = -vx*1.3;
        stupidshit();
      } else {
        rightScore++;
        reset();
      }
    }
    if (x+size > rightPaddle.x) {
      if (y > rightPaddle.y &&
          y < rightPaddle.y + rightPaddle.h) {
        vx = -vx*1.3;
        stupidshit();
      } else {
        leftScore++;
        reset();
      }
    }
  }
}

class Paddle {
  float x, y;
  float vy;
  float w = 15, h = 80;
  color c;

  Paddle(float x, float y, color c) {
    this.x = x;
    this.y = y;
    this.c = c;
    vy = 0;
  }

  void draw() {
    noStroke();
    fill(c);
    rect(x, y, w, h);
  }

  void move() {
    y += vy;
    if (y < 0 || y > height-h) {
      vy = 0;
      y = constrain(y, 0, height-h);
    }
  }
}

ArrayList<Ball> list;
Paddle leftPaddle, rightPaddle;
int leftScore = 0;
int rightScore = 0;

void setup() {
  size(500, 500);
  colorMode(HSB);
  list = new ArrayList<Ball>();
  list.add(new Ball(width/2, height/2, color(random(255), 255, 255)));

  leftPaddle = new Paddle(15, 150, color(0));
  rightPaddle = new Paddle(width-15-15, 150, color(0));
}

void drawScores() {
  textSize(32);
  text(""+leftScore, width/2-30, 40);
  text(""+rightScore, width/2+10, 40);
}

void draw() {
  background(255);

  for (int i = 0; i < list.size(); i += 1) {
    list.get(i).draw();
    list.get(i).move();
  }

  leftPaddle.move();
  leftPaddle.draw();
  rightPaddle.move();
  rightPaddle.draw();
  drawScores();
}

void mousePressed() {
  list.add(new Ball(mouseX, mouseY, color(random(255), 255, 255)));
}

void keyPressed() {
  if (key == 'q') {
    leftPaddle.vy = -4;
  }
  if (key == 'a') {
    leftPaddle.vy = 4;
  }
  if (key == 'o') {
    rightPaddle.vy = -4;
  }
  if (key == 'l') {
    rightPaddle.vy = 4;
  }
}

void keyReleased() {
  if (key == 'q' && leftPaddle.vy < 0) {
    leftPaddle.vy = 0;
  }
  if (key == 'a' && leftPaddle.vy > 0) {
    leftPaddle.vy = 0;
  }
  if (key == 'o' && rightPaddle.vy < 0) {
    rightPaddle.vy = 0;
  }
  if (key == 'l' && rightPaddle.vy > 0) {
    rightPaddle.vy = 0;
  }
}
