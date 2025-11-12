# Distributed Interaction

**NAMES OF COLLABORATORS HERE**

For submission, replace this section with your documentation!

---

## Prep

1. Pull the new changes
2. Read: [The Presence Table](https://dl.acm.org/doi/10.1145/1935701.1935800) ([video](https://vimeo.com/15932020))

## Overview

Build interactive systems where **multiple devices communicate over a network** using MQTT messaging. Work in teams of 3+ with Raspberry Pis.

**Parts:**
- A: Learn MQTT messaging
- B: Try collaborative pixel grid demo  
- C: Build your own distributed system

---

## Part A: MQTT Messaging
<details>
<summary>Click to expand</summary>
 
MQTT = lightweight messaging for IoT. Publish/subscribe model with central broker.

**Concepts:**
- **Broker**: `farlab.infosci.cornell.edu:1883`
- **Topic**: Like `IDD/bedroom/temperature` (use `#` wildcard)
- **Publish/Subscribe**: Send and receive messages

**Install MQTT tools on your Pi:**
```bash
sudo apt-get update
sudo apt-get install -y mosquitto-clients
```

**Test it:**

**Subscribe to messages (listener):**
```bash
mosquitto_sub -h farlab.infosci.cornell.edu -p 1883 -t 'IDD/#' -u idd -P 'device@theFarm'
```

**Publish a message (sender):**
```bash
mosquitto_pub -h farlab.infosci.cornell.edu -p 1883 -t 'IDD/test/yourname' -m 'Hello!' -u idd -P 'device@theFarm'
```

> **💡 Tips:**
> - Replace `yourname` with your actual name in the topic
> - Use single quotes around the password: `'device@theFarm'`

**🔧 Debug Tool:** View all MQTT messages in real-time at `http://farlab.infosci.cornell.edu:5001`

![MQTT Explorer showing messages](imgs/MQTT-explorer.png)

**💡 Brainstorm 5 ideas for messaging between devices**
</details>
---

## Part B: Collaborative Pixel Grid
<details>
<summary>Click to expand</summary>

 Each Pi = one pixel, controlled by RGB sensor, displayed in real-time grid.

**Architecture:** `Pi (sensor) → MQTT → Server → Web Browser`

**Setup:**

1. **Sensor**

#### Light/Proximity/Gesture sensor (APDS-9960)
We use this sensor [Adafruit APDS-9960](https://www.adafruit.com/product/3595) for this exmaple to detect light (also RGB)
 
<img src="https://cdn-shop.adafruit.com/970x728/3595-06.jpg" width=200>

Connect it to your pi with Qwiic connector


<img src="imgs/IMG_0270.jpg" height="200" />
We need to use the screen to display the color detection, so we need to stop the running piscreen.service to make your screen available again

```bash
# stop the screen service
sudo systemctl stop piscreen.service
```

if you want to restart the screen service
```bash
# start the screen service
sudo systemctl start piscreen.service
```
 
2. **Server** (one person on laptop):
```bash
cd "Lab 6"  
source .venv/bin/activate
pip install -r requirements-server.txt
python app.py
```

2. **View in browser:**
   - Grid: `http://farlab.infosci.cornell.edu:5000`
   - Controller: `http://farlab.infosci.cornell.edu:5000/controller`

3. **Pi publisher** (everyone on their Pi):
```bash
# First time setup - create virtual environment
cd "Lab 6"
python -m venv .venv
source .venv/bin/activate
pip install -r requirements-pi.txt

# Run the publisher
python pixel_grid_publisher.py
```

Hold colored objects near sensor to change your pixel!

![Pixel grid with two devices](imgs/two-devices-grid.png)

**📸 Include: Screenshot of grid + photo of your Pi setup**
![IMG_4089](https://github.com/user-attachments/assets/96b94408-ce3a-43aa-9e52-4af26a549506)
![IMG_4090](https://github.com/user-attachments/assets/8df96a70-b482-4473-9638-a3ada7e34484)


https://github.com/user-attachments/assets/d628ca74-17f0-46ca-a328-68110f77c02f
</details>

---

## Part C: Make Your Own
<details>
<summary>Click to expand</summary>
 
**Requirements:**
- 3+ people, 3+ Pis
- Each Pi contributes sensor input via MQTT
- Meaningful or fun interaction

**Ideas:**

**Sensor Fortune Teller**
- Each Pi sends 0-255 from different sensor
- Server generates fortunes from combined values

**Frankenstories**
- Sensor events → story elements (not text!)
- Red = danger, gesture up = climbed, distance <10cm = suddenly

**Distributed Instrument**
- Each Pi = one musical parameter
- Only works together

**Others:** Games, presence display, mood ring

### Deliverables

Replace this README with your documentation:
</details>
**1. Project Description**
### What does it do?

The Distributed Goose Counting Game is a collaborative counting challenge where multiple players use Raspberry Pi devices with physical buttons to compete in counting geese from an image. The system creates an engaging, real-time competitive experience using distributed hardware and networked communication.

**Game Flow:**
1. An image of geese is displayed for 5 seconds
2. After the image disappears, players wait through a 5-second countdown
3. When the countdown ends, players have 5 seconds to answer
4. Players press their button once for each goose they counted
5. After 5 seconds, the system automatically calculates each player's answer based on their total button presses
6. The first player with the correct answer wins

### Why is it interesting?

This project demonstrates distributed sensing and real-time collaboration in several ways:

**Real-time Feedback:** After the 5-second answer period, all players' answers are revealed simultaneously on the web interface, creating anticipation and excitement.

**Physical-Digital Bridge:** The game combines physical button pressing with digital visualization, making abstract networked systems tangible and fun.

**Collaborative Competition:** While players compete individually, the system only works when multiple people participate together, demonstrating the power of networked devices.

### User Experience

Players experience the game in three phases:

**Observation Phase:** Tension builds as players quickly count geese while the image is visible. The time pressure creates urgency.

**Waiting Phase:** The countdown creates anticipation. Players must remember their count while watching the timer.

**Action Phase:** Players frantically press buttons to enter their count within the 5-second window. They cannot see other players' progress during this time, adding tension. When time expires, all answers are revealed simultaneously, creating a dramatic moment of truth.

The physical button creates a more engaging experience than keyboard input - each press feels meaningful and the pressure of the ticking clock creates urgency.

---

**2. Architecture Diagram**
- Hardware, connections, data flow
- Label input/computation/output

**3. Build Documentation**
- Photos of each Pi + sensors
- MQTT topics used
- Code snippets with explanations

**4. User Testing**
- **Test with 2+ people NOT on your team**
- Photos/video of use
- What did they think before trying?
- What surprised them?
- What would they change?

We tested our **Bird Counting Game** with **three players who were not on our team**. Each participant used a Raspberry Pi with a physical button and LED, and all shared the same web interface displayed on a laptop screen.

Before Trying

Before playing, participants expected the game to be simple—just pressing a button when prompted. They didn’t realize it would require both **memorizing the number of birds** and **reacting precisely after the countdown**, so they thought it might just test reaction speed.

What Surprised Them

They were surprised by how challenging the game felt. The first version only displayed the bird image for **3 seconds**, which made it difficult to finish counting. After we extended the display to **5 seconds**, players said it finally felt fair while still exciting.
They also mentioned that the **five-second countdown** built strong anticipation—the moment when “Go!” appeared created real tension, and they instinctively tried to react as fast as possible.

What They Would Change

Participants suggested:

* Adding **sound cues** (for example, a short beep each second during the countdown and a chirp sound on “Go!”).
* Showing a **visual leaderboard** that tracks each player’s fastest reaction time and number of correct rounds.
* Adding a quick replay or “next round” animation to make transitions smoother.

### 🎥 Photos / Video

We recorded a short clip (`output_small.mp4`) and took photos of the players during testing, showing the interface, button presses, and LED feedback across multiple Pis.

---

是否希望我帮你再写一版简短版（像课堂示例那样 4-5 行）以备 README 排版更紧凑？

**5. Reflection**
- What worked well?
- Challenges with distributed interaction?
- How did sensor events work?
- What would you improve?

---

## Code Files

**Server files:**
- `app.py` - Pixel grid server (Flask + WebSocket + MQTT)
- `mqtt_viewer.py` - MQTT message viewer for debugging
- `mqtt_bridge.py` - MQTT → WebSocket bridge
- `requirements-server.txt` - Server dependencies

**Pi files:**
- `pixel_grid_publisher.py` - Example (RGB sensor → MQTT)
- `requirements-pi.txt` - Pi dependencies

**Web interface:**
- `templates/grid.html` - Pixel grid display
- `templates/controller.html` - Color picker
- `templates/mqtt_viewer.html` - Message viewer

---

## Debugging Tools

**MQTT Message Viewer:** `http://farlab.infosci.cornell.edu:5001`
- See all MQTT messages in real-time
- View topics and payloads
- Helpful for debugging your own projects

**Command line:**
```bash
# See all IDD messages
mosquitto_sub -h farlab.infosci.cornell.edu -p 1883 -t "IDD/#" -u idd -P "device@theFarm"
```

---

## Troubleshooting

**MQTT:** Broker `farlab.infosci.cornell.edu:1883`, user `idd`, pass `device@theFarm`

**Sensor:** Check `i2cdetect -y 1`, APDS-9960 at `0x39`

**Grid:** Verify server running, check MQTT in console, test with web controller

**Pi venv:** Make sure to activate: `source .venv/bin/activate`


---

## Submission Checklist

Before submitting:
- [ ] Delete prep/instructions above
- [ ] Add YOUR project documentation
- [ ] Include photos/videos/diagrams  
- [ ] Document user testing with non-team members
- [ ] Add reflection on learnings
- [ ] List team names at top

**Your README = story of what YOU built!**

---

Resources: [MQTT Guide](https://www.hivemq.com/mqtt-essentials/) | [Paho Python](https://www.eclipse.org/paho/index.php?page=clients/python/docs/index.php) | [Flask-SocketIO](https://flask-socketio.readthedocs.io/)
