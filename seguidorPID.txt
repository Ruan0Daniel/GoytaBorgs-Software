#define sensor1 A0 //l
#define sensor2 A1
#define sensor3 A2
#define sensor4 A3 //r
#define motorL 5
#define motorR 6
#define ativaMotores 11

float speedBase = 0;
float kp = 0;
float ki = 0;
float kd = 0;

float pesoL;
float pesoR;
float erro;
float integral;
float derivada;
float correcao;
float lerro;

void setup() {
  pinMode(sensor1, INPUT);
  pinMode(sensor2, INPUT);
  pinMode(motorL, OUTPUT);
  pinMode(motorR, OUTPUT);
  pinMode(ativaMotores, OUTPUT);
  
  digitalWrite(ativaMotores, HIGH);
}

void loop() {
  erro = round(analogRead(sensor2/10.23)) - round(analogRead(sensor3/10.23));
  integral += erro;
  derivada = erro - lerro;
  correcao = (erro*kp) + (integral*ki) + (derivada*kd);
  analogWrite(motorL, ( (speedBase*pesoL) + correcao) );
  analogWrite(motorR, ( (speedBase*pesoR) - correcao) );
  lerro = erro;
  
  if (round(analogRead(sensor1/10.23))<50) {
    pesoL = 0.7;
    pesoR = 0.3;
  }
  if (round(analogRead(sensor4/10.23))<50) {
    pesoL = 0.3;
    pesoR = 0.7;
  }
  else {
    pesoL = 0.5;
    pesoR = 0.5;
  }

}
