# Automated-super-market-trolley-
An automated supermarket trolley is a smart shopping cart equipped with advanced technology such as sensors, cameras, and AI. It assists customers by navigating the store, locating items, and automatically scanning and tallying products placed in the cart. 
// Define pins for motor driver
const int motor1Pin1 = 2;
const int motor1Pin2 = 3;
const int motor2Pin1 = 4;
const int motor2Pin2 = 5;
const int enablePin1 = 9;
const int enablePin2 = 10;

// Define pins for ultrasonic sensor
const int trigPin = 6;
const int echoPin = 7;

// Variables for ultrasonic sensor
long duration;
int distance;

// Motor speed
const int motorSpeed = 200;

void setup() {
  // Set motor pins as output
  pinMode(motor1Pin1, OUTPUT);
  pinMode(motor1Pin2, OUTPUT);
  pinMode(motor2Pin1, OUTPUT);
  pinMode(motor2Pin2, OUTPUT);
  pinMode(enablePin1, OUTPUT);
  pinMode(enablePin2, OUTPUT);
  
  // Set ultrasonic sensor pins
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  
  // Initialize serial communication
  Serial.begin(9600);
}

void loop() {
  // Measure distance
  distance = getDistance();
  Serial.print("Distance: ");
  Serial.println(distance);

  if (distance < 20) {
    // Stop the trolley if an obstacle is detected within 20 cm
    stopTrolley();
  } else {
    // Move the trolley forward
    moveForward();
  }
  
  delay(100);
}

void moveForward() {
  // Set motor speed
  analogWrite(enablePin1, motorSpeed);
  analogWrite(enablePin2, motorSpeed);
  
  // Move both motors forward
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);
}

void stopTrolley() {
  // Stop both motors
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, LOW);
  digitalWrite(motor2Pin2, LOW);
}

int getDistance() {
  // Send a 10us pulse to trigger pin
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // Read the echo pin
  duration = pulseIn(echoPin, HIGH);
  
  // Calculate the distance
  distance = duration * 0.034 / 2;
  
  return distance;
}
