let pacMan;
let grid = 20;
let walls = [];

function setup() {
  createCanvas(400, 400);
  pacMan = new PacMan();
  
  // Criação do cenário (paredes)
  walls.push(new Wall(0, 0, width, grid)); // Topo
  walls.push(new Wall(0, 0, grid, height)); // Esquerda
  walls.push(new Wall(width - grid, 0, grid, height)); // Direita
  walls.push(new Wall(0, height - grid, width, grid)); // Fundo
  
  // Adicione mais paredes conforme necessário
}

function draw() {
  background(0);
  
  // Desenha as paredes
  for (let wall of walls) {
    wall.show();
  }
  
  pacMan.update();
  pacMan.show();
}

function keyPressed() {
  if (keyCode === UP_ARROW) {
    pacMan.setDirection(0, -1);
  } else if (keyCode === DOWN_ARROW) {
    pacMan.setDirection(0, 1);
  } else if (keyCode === LEFT_ARROW) {
    pacMan.setDirection(-1, 0);
  } else if (keyCode === RIGHT_ARROW) {
    pacMan.setDirection(1, 0);
  }
}

class PacMan {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
    this.xspeed = 0;
    this.yspeed = 0;
    this.radius = grid / 2;
  }
  
  update() {
    // Movimento do Pac-Man
    this.x += this.xspeed * grid;
    this.y += this.yspeed * grid;
    
    // Verifica colisão com as paredes
    for (let wall of walls) {
      if (this.collide(wall)) {
        // Impede o movimento se houver colisão
        this.x -= this.xspeed * grid;
        this.y -= this.yspeed * grid;
      }
    }
  }
  
  show() {
    fill(255, 255, 0);
    noStroke();
    arc(this.x, this.y, this.radius * 2, this.radius * 2, QUARTER_PI + HALF_PI * this.xspeed, TWO_PI - QUARTER_PI + HALF_PI * this.xspeed, PIE);
  }
  
  setDirection(x, y) {
    this.xspeed = x;
    this.yspeed = y;
  }
  
  collide(wall) {
    // Verifica se o Pac-Man colide com a parede
    let halfWidth = wall.w / 2;
    let halfHeight = wall.h / 2;
    let closestX = constrain(this.x, wall.x - halfWidth, wall.x + halfWidth);
    let closestY = constrain(this.y, wall.y - halfHeight, wall.y + halfHeight);
    let distanceX = this.x - closestX;
    let distanceY = this.y - closestY;
    let distanceSquared = distanceX * distanceX + distanceY * distanceY;
    return distanceSquared < (this.radius * this.radius);
  }
}

class Wall {
  constructor(x, y, w, h) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
  }
  
  show() {
    fill(255);
    rect(this.x, this.y, this.w, this.h);
  }
}
