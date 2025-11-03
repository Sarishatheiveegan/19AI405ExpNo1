<h1>ExpNo 1 :Developing AI Agent with PEAS Description</h1>
<h3>Name: MARINO SARISHA T</h3>
<h3>Register Number: 212223240084</h3>


<h3>AIM:</h3>
<br>
<p>To find the PEAS description for the given AI problem and develop an AI agent.</p>
<br>
<h3>Theory</h3>
<h3>Medicine prescribing agent:</h3>
<p>Such this agent prescribes medicine for fever (greater than 98.5 degrees) which we consider here as unhealthy, by the user temperature input, and another environment is rooms in the hospital (two rooms). This agent has to consider two factors one is room location and an unhealthy patient in a random room, the agent has to move from one room to another to check and treat the unhealthy person. The performance of the agent is calculated by incrementing performance and each time after treating in one room again it has to check another room so that the movement causes the agent to reduce its performance. Hence, agents prescribe medicine to unhealthy.</p>
<hr>
<h3>PEAS DESCRIPTION:</h3>
<table>
  <tr>
    <td><strong>Agent Type</strong></td>
    <td><strong>Performance</strong></td>
     <td><strong>Environment</strong></td>
    <td><strong>Actuators</strong></td>
    <td><strong>Sensors</strong></td>
  </tr>
    <tr>
    <td><strong>Medicine prescribing agent</strong></td>
    <td><strong>Treating unhealthy, agent movement</strong></td>
     <td><strong>Rooms, Patient</strong></td>
    <td><strong>Medicine, Treatment</strong></td>
    <td><strong>Location, Temperature of patient</strong></td>
  </tr>
</table>
<hr>
<H3>DESIGN STEPS</H3>
<h3>STEP 1:Identifying the input:</h3>
<p>Temperature from patients, Location.</p>
<h3>STEP 2:Identifying the output:</h3>
<p>Prescribe medicine if the patient in a random has a fever.</p>
<h3>STEP 3:Developing the PEAS description:</h3>
<p>PEAS description is developed by the performance, environment, actuators, and sensors in an agent.</p>
<h3>STEP 4:Implementing the AI agent:</h3>
<p>Treat unhealthy patients in each room. And check for the unhealthy patients in random room</p>
<h3>STEP 5:</h3>
<p>Measure the performance parameters: For each treatment performance incremented, for each movement performance decremented</p>

## PROGRAM
```
import random
import time

class Thing:
    def is_alive(self): return hasattr(self, "alive") and self.alive

class Agent(Thing):
    def __init__(self, program=None): self.alive, self.performance, self.program = True, 0, program

def TableDrivenAgentProgram(table):
    percepts = []
    def program(percept): percepts.append(percept); return table.get(tuple(percepts))
    return program

room_A, room_B = (0,0), (1,0)

def TableDrivenDoctorAgent():
    table = {((room_A, "healthy"),): "Right", ((room_A, "unhealthy"),): "treat",
             ((room_B, "healthy"),): "Left", ((room_B, "unhealthy"),): "treat"}
    return Agent(TableDrivenAgentProgram(table))

class Environment:
    def __init__(self): self.things, self.agents = [], []
    def is_done(self): return not any(a.is_alive() for a in self.agents)
    def step(self):
        if not self.is_done():
            actions = [a.program(self.percept(a)) for a in self.agents if a.alive]
            for agent, action in zip(self.agents, actions): self.execute_action(agent, action)
    def run(self, steps=1000):
        for _ in range(steps):
            if self.is_done(): return
            self.step()
    def add_thing(self, thing, location=None):
        if not isinstance(thing, Thing): thing = Agent(thing)
        if thing not in self.things:
            thing.location = location or self.default_location(thing)
            self.things.append(thing)
            if isinstance(thing, Agent): self.agents.append(thing)

class TrivialDoctorEnvironment(Environment):
    def __init__(self):
        super().__init__()
        self.status = {room_A: random.choice(["healthy", "unhealthy"]), room_B: random.choice(["healthy", "unhealthy"])}
    def percept(self, agent): return agent.location, self.status[agent.location]
    def execute_action(self, agent, action):
        if action in ["Right", "Left"]:
            agent.location = room_B if action == "Right" else room_A
            agent.performance -= 1
        elif action == "treat":
            tem = float(input("Enter your temperature: "))
            if tem >= 98.5: print("Medicine: Paracetamol & Low-dose Antibiotic")
            self.status[agent.location] = "healthy"
            agent.performance += 10
    def default_location(self, thing): return random.choice([room_A, room_B])

if __name__ == "__main__":
    agent = TableDrivenDoctorAgent()
    env = TrivialDoctorEnvironment()
    env.add_thing(agent)
    print("\tBefore Treatment: ", env.status, "Agent Location:", agent.location, "Performance:", agent.performance)
    time.sleep(2)
    for _ in range(2):
        env.run(1)
        print("\tAfter Treatment: ", env.status, "Agent Location:", agent.location, "Performance:", agent.performance)
        time.sleep(2)
```

## OUTPUT:
![Screenshot 2025-03-26 111335](https://github.com/user-attachments/assets/a4611918-08c1-487c-8be3-5d5df3d824f5)

## RESULT:
Thus the program executed successfully.
