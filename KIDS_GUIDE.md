# Welcome to Your Robot!

Have you ever wondered how a robot vacuum knows when to turn before it bumps into the couch? Or how the robot arms in factories pick up tiny parts without dropping them? You're about to find out, because the little car you just built does something very similar. This guide walks you through what's happening inside it: the brain, the senses, the muscles, and the secret way they all talk to each other.

It's written for curious 10–12 year olds. If you've ever wondered how a machine can "decide" something on its own, you're going to like this.

---

## 1. What is a robot?

A robot is a machine that does three things, in this order, over and over again:

```
       +-----------+
       |  SENSE    |   (look around)
       +-----------+
             |
             v
       +-----------+
       |  THINK    |   (decide what to do)
       +-----------+
             |
             v
       +-----------+
       |  ACT      |   (move, turn, light up)
       +-----------+
             |
             '----> back to SENSE...
```

That loop is the secret to *every* robot in the world. A factory arm welding a car, a Mars rover dodging rocks, a Roomba sliding under the sofa — they all sense, then think, then act, then sense again. Hundreds of times a second.

Here's an important question: how is a robot different from a remote-control toy? After all, both move when you press buttons. The difference is the **think** step. A remote-control car only does what *you* tell it. Take your hands off the controls and it stops being interesting. But a robot can be left alone, and it will keep sensing, thinking, and acting all by itself.

Your robot car has a "Standby" mode where it just sits there waiting (boring, on purpose). It also has an "Obstacle Avoidance" mode where it drives around your room *by itself*, finding open paths and turning away from walls. When that's happening, it's being a real robot.

---

## 2. Meet the brain: Arduino

If you flip your car upside down (carefully!), you'll see a small blue circuit board with the word **Arduino** printed on it. That little board is the robot's brain.

Here's the funny thing about an Arduino: out of the box, it does *nothing*. Plug it in, turn it on, and it just sits there. It's like a brain with no memories. Boring.

What turns it from boring into magical is **programming**. You write a list of instructions on your laptop ("when X happens, do Y") and send them to the Arduino over a USB cable. The Arduino remembers your instructions — even after you unplug everything — and from then on, every time you turn the robot on, it follows them.

The Arduino chip in your robot is about the size of your fingernail and runs at 16 MHz. That sounds fast, but your phone is about a thousand times faster. The cool part is that the Arduino doesn't *need* to be fast. Detecting a wall and turning a wheel doesn't take a lot of computer power — it just takes the right instructions, in the right order.

---

## 3. Senses and muscles

For the SENSE / THINK / ACT loop to work, your robot needs ways to feel the world (sensors) and ways to push back on the world (motors and lights). Here's the rough mapping between you and your robot:

```
   YOU                       YOUR ROBOT
   --------                  -------------------
   eyes & ears   ---------->  sensors
   brain         ---------->  Arduino UNO
   arms & legs   ---------->  motors
   voice         ---------->  RGB LEDs ("talking back" with color)
```

Your robot has several kinds of sensors, and each one does something different:

- **Ultrasonic sensor** — the thing on the front that looks like two eyes. It "hears" by bouncing sound off whatever's in front of it. We'll see how this works in section 5; it's the coolest one.
- **Line-tracking sensors** — three little eyes underneath, looking down. They can tell whether the floor below is light or dark, which is how the robot follows a black line of tape.
- **Gyroscope** — a tiny chip on the main board. It tells the brain how the robot is tilted and how fast it's turning. This is what your inner ear does for *you* when you spin in a chair.
- **IR receiver** — the little dome that watches the remote. It listens for invisible flashes from the remote control.
- **Button** — on top. The simplest sensor of all: either pressed, or not pressed.

And on the "muscles" side:

- **Two motors** that turn the wheels. They can spin forward, backward, or stop, at different speeds.
- **Two small servos** that turn the front "head" left/right and up/down. Servos are like motors, but instead of just spinning, they go to a *specific* angle when you tell them to.
- **RGB LEDs** that light up in any color. Not really a muscle, but it's how the robot can communicate back to you.

---

## 4. How you tell the robot what to do

A program is just a list of instructions. The tricky part is that computers are *extremely* literal. If you tell a person "turn left," they get it. If you tell a robot "turn left," it'll ask: "How much? At what speed? For how long? With which wheel?" You have to spell out every detail.

Programs for Arduino are written in a language called **C++**. Don't be intimidated by the name — you only need a tiny bit of it to make a robot do things.

Every Arduino program has two special functions, and they always do the same job:

```
   POWER ON
       |
       v
   +-----------+
   | setup()   |   runs ONCE
   +-----------+
       |
       v
   +-----------+
   |  loop()   |   runs again
   |           |   and again
   |           |   and again...
   +-----------+
       |
       '------ (forever, until you turn it off)
```

Here's the world's simplest Arduino program. It blinks the little LED that's built into the board:

```cpp
void setup() {
  pinMode(13, OUTPUT);     // Tell pin 13 it'll be controlling something.
}

void loop() {
  digitalWrite(13, HIGH);  // Turn the LED on.
  delay(500);              // Wait half a second.
  digitalWrite(13, LOW);   // Turn the LED off.
  delay(500);              // Wait half a second.
}                          // Then loop() runs again, forever.
```

That's a complete, working Arduino program. Five lines of useful code. The robot car's program is much longer — around 2000 lines — but it's built out of the same kind of pieces: telling pins what to do, waiting, and reading or writing values.

---

## 5. What happens inside the brain when it sees a wall

Let's zoom in on one specific moment. Your robot is in Obstacle Avoidance mode, rolling forward, and there's a wall about 20 cm in front of it. What's happening inside?

It starts with the **ultrasonic sensor** — those two "eyes" on the front. They're not really eyes; they're a tiny speaker and a tiny microphone. The brain tells the speaker to make a *very* short, *very* high-pitched chirp — so high you can't even hear it. The chirp travels through the air at about 343 meters per second (the speed of sound). When it hits the wall, it bounces back, like a ball thrown against a garage door. The microphone hears the echo come back.

```
   YOUR ROBOT                          WALL
   +------+                             |
   | o  o |  ----- ping! ------>        |
   +------+                             |
                                        |
   +------+                             |
   | o  o | <----- echo! -------        |
   +------+                             |
```

Now the clever bit: the brain measures *how long* it took for the echo to come back. The longer the echo takes, the farther away the wall is. There's even a little formula:

> distance in centimeters = (echo time in microseconds) ÷ 58

The 58 comes from the speed of sound and the fact that the chirp travels there *and* back (round trip). You don't have to memorize it — just know that whenever someone uses an ultrasonic sensor, they divide by 58 to get centimeters.

So now the brain knows: "Wall is 19 cm away." That's less than 20 cm, which is the threshold for "blocked." Time to think.

The brain doesn't just panic and turn left. It does something smarter: it points the sensor head to the **right** (30 degrees), takes a reading, points it **forward** (90 degrees), takes another reading, then points it to the **left** (150 degrees) and takes a third. Now it has three distances. It picks whichever direction has the *most* open space and turns that way.

If *all three* directions are blocked — it's in a corner — it backs up a bit and then turns in a random direction. The randomness matters: if it always turned right, and right was a wall too, it would get stuck doing the same dance forever, like a person trying to leave a room by walking into the same wall over and over.

If you ever want to find this code, look for `ApplicationFunctionSet_Obstacle` inside `ApplicationFunctionSet_xxx0.cpp`. That single function is the robot's whole "what do I do when I see a wall?" decision, and now you know what every line is trying to do.

---

## 6. Try it yourself

Reading is fun. Doing is more fun. Here are three experiments, easiest first:

**Experiment 1: Drive at different speeds.**
Put the robot in Rocker (joystick) mode using the phone app. Drive it slowly toward a wall, then quickly. Does it react the same way at both speeds? Why might fast driving be harder for the robot? (Hint: think about how long it takes to listen for an echo.)

**Experiment 2: Switch modes with the remote.**
Press button **1** on the IR remote: line-tracking mode (put it on a black tape line on the floor). Press button **2**: obstacle-avoidance mode (let it loose in your room). Press button **3**: follow mode (it'll follow your hand). Watch how the same hardware does totally different things just because the brain is running different instructions.

**Experiment 3: Change a number in the code (advanced).**
Open `ApplicationFunctionSet_xxx0.cpp` in the Arduino IDE. Find the line that says `BLOCKED_CM = 20`. Change the 20 to a 40, then upload. Now the robot will stop and start scanning when it sees a wall *40* centimeters away instead of 20 — it'll be more cautious. Don't worry about breaking anything: if something goes weird, change the number back to 20 and upload again. Every robot programmer in the world learns by changing numbers and seeing what happens.

---

That's it — you now know the basics of what your robot is, what it can sense, and how it thinks. The next time you see a robot vacuum bumping its way around your living room, you'll know exactly what's going on inside its head.
