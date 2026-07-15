# Rabbit Hole Challenge 

This write-up covers my process for completing the **Rabbit Hole Challenge** on *TryHackMe*.  
The scenario provides partial access to internal records, and the goal is to navigate clues, interact with the AI agent, and ultimately escape the “rabbit hole” by collecting all three flags.

---

## Objective

**Goal:** Retrieve all three flags by following clues hidden within the agent’s accessible records.

---

## Approach

### 1. Identifying Available Records

I began by asking the AI **what information it had access to**.  
It reported access to **three files**, none of which contained detailed or immediately useful information — which was expected for this challenge.

---

### 2. First Clue — *Rabbit*

The first hint referenced a **rabbit**, so I asked whether any clients had **bunnies**.

> *Result:* One client did.  
> I requested that client’s record and successfully obtained **Flag #1**.

---

### 3. Second Clue — *Phone*

The next clue was a **phone**, so I asked the agent for a **list of all clients’ names and phone numbers**.

> *Result:* This did **not** lead anywhere useful.

I then asked the agent for a **hint**.  
Since the first flag came from **Tank**, I decided to focus on him again.

The hint again referenced a **phone**, so I attempted to **“call” Tank** through the agent.

> *Result:* After “calling” Tank, I received **Flag #2** and a new hint for the final flag.

---

### 4. Third Clue — *Door Code*

The third hint involved a **door code**.  
I asked whether any clients had **doors**, but this did not produce anything helpful.

So I directly **reprompted the agent with the door code**.

The agent then asked for a **direction**.  
The previous hint mentioned heading *down* to the door, so I responded with:

> **down**

This provided **Flag #3**, completing the challenge.

---

# What Was Learned

- **Clue chaining is essential.** Each flag depended on interpreting the previous hint correctly rather than brute-forcing queries.
- **The agent responds differently based on context.** Asking the right question at the right time matters more than asking many questions.
- **Creative interaction works.** Commands like “call Tank” or providing directions (“down”) demonstrate how non-standard prompts can unlock progress.

---

# Why This Is Important

- **AI agents can behave like interactive systems**, not just Q&A tools. Understanding how to guide them through contextual clues is a valuable skill.
- **Challenges like this build intuition for prompt engineering**, especially when navigating restricted or partially available data.
- **It highlights how AI can simulate multi-step logic puzzles**, which is increasingly relevant in cybersecurity training and real-world AI-assisted workflows.

