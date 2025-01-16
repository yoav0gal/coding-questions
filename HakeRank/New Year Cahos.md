> I approached this problem by simulating the bribing process. Here's the breakdown:

1. **Initial Queue:** I created an `initialQueue` to represent the original order of people.
2. **Iteration and Comparison:** I iterated through the given queue, comparing each person's current position to their original position.
3. **Bribe Scenarios:**
   - **No Bribe:** The person is in their original position.
   - **Bribed Once:** The person is one position ahead (e.g., original position 3, current position 2).
   - **Bribed Twice:** The person is two positions ahead (e.g., original position 4, current position 2).
   - **Too Chaotic:** The person is more than two positions ahead.

**Why Swapping?**

> The core of my solution involves swapping elements in the `initialQueue`. This is crucial because it allows me to:

- **Track Bribes:** By swapping elements, I effectively simulate the bribing process and keep track of how the queue changes with each bribe.
- **Maintain Accuracy:** Swapping ensures that the `initialQueue` always reflects the state of the queue *after* a bribe has occurred. This is vital for correctly identifying subsequent bribes.

**Example:**

Let's say the original queue is `[1, 2, 3, 4]` and the current queue is `[3, 1, 2, 4]`. Here's how swapping helps:

1. **Initial State:** `initialQueue = [1, 2, 3, 4]`
2. **Person 3 bribed twice:** To get to the front, person 3 must have bribed both person 2 and person 1.
3. **Swap 1:** To simulate the first bribe, we swap person 3 and person 2 in the `initialQueue`: `[1, 3, 2, 4]`.
4. **Swap 2:** To simulate the second bribe, we swap person 3 and person 1: `[3, 1, 2, 4]`.

Now the `initialQueue` matches the `currentQueue`, and we've accurately tracked the two bribes.

<details>
<summary> <b>Code:</b> </summary>

```typescript
function minimumBribes(q: number[]): void {
    // Create an array representing the initial queue, where everyone is in their original position.
    const initialQueue = q.map((value, index) => index + 1);
    
    let minBribes = 0; // Initialize a counter for the minimum bribes

    // Iterate through the given queue (q)
    for (let i = 0; i < q.length; i++) {
        const currentPerson = q[i];  // The person's current position in the queue
        const initialPerson = initialQueue[i]; // The person's original position in the queue
        const nextInitialPerson = i + 1 < initialQueue.length ? initialQueue[i + 1] : null; // The person originally behind the current person
        const next2initialPerson = i + 2 < initialQueue.length ? initialQueue[i + 2] : null; // The person two positions behind the current person

        // If the person is in their original position, no bribe occurred.
        if (currentPerson == initialPerson) continue;

        // If the person bribed the one directly in front of them:
        if (currentPerson == nextInitialPerson) {   
            // Swap the positions in the initialQueue to reflect the bribe
            initialQueue[i + 1] = initialPerson; // This was incorrect in the previous version
            initialQueue[i] = nextInitialPerson;
            minBribes += 1; // Increment the bribe counter
            continue; // Move to the next person
        }

        // If the person bribed the two people in front of them:
        if (currentPerson == next2initialPerson) {
            // Swap the positions in the initialQueue to reflect the bribes
            initialQueue[i + 2] = nextInitialPerson; 
            initialQueue[i + 1] = initialPerson; 
            initialQueue[i] = next2initialPerson;
            minBribes += 2; // Increment the bribe counter by 2
            continue; // Move to the next person
        }

        // If the person bribed more than two people, the queue is too chaotic.
        return console.log("Too chaotic"); 
    }

    // If the queue is not chaotic, print the minimum number of bribes.
    console.log(minBribes);
}
```

</details>
