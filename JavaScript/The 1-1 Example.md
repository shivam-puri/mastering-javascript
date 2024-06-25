```javascript
  

/* This Node program reads text from standard input, computes
the frequency * of each letter in that text, and displays a histogram of the
most frequently used characters. It requires Node 12 or higher to
run.
*/

// This class extends Map so that the get() method returns the specified value instead of null when the key is not in the map
class DefaultMap extends Map {
    constructor(defaultValue) {
        super();       // super is the constructor of parent class i.e. Map here
        this.defaultValue = defaultValue; // Remember the default value so that when you try to retrieve a value for a key that doesn't exist in the `DefaultMap`, it likely retrieves this stored `defaultValue` instead of throwing an error, as a regular `Map` would do.
    }
    get(key) {
        if (this.has(key)) {            // If the key is already in map
            return super.get(key);    // return its value from superclass
        }
        else {
            return this.defaultValue;  // otherwise return the default value
        }
    }

}

// This class computes and displays letter frequency histograms
class Histogram {
    constructor() {
        this.letterCounts = new DefaultMap(0);
        this.totalLetters = 0;
    }

    // This function updates the histogram with letters of text
    add(text) {
        // Remove whitespaces and convert to uppercase
        text = text.replace(/\s/, "").toUpperCase();
        // Now loop through the characters of text
        for (let character of text) {
            // retrieve the current count for the current character
            // from the `letterCounts` map. Since `letterCounts` is
            // a `DefaultMap` initialized with a default value of `0`, it will
            // return `0` if the character hasn't been encountered before.
            let count = this.letterCounts.get(character);
            // Update the count for the current character
            this.letterCounts.set(character, count + 1);
            // Updating the totalLetters by 1
            this.totalLetters++;

        }

    }

    // Convert the histogram to a string that displays an ASCII graphic
    toString() {

        // convert map to an array of [key, value] arrays
        let entries = [...this.letterCounts];
        // sort the array by count, then alphabetically
        // a and b are any two [key, value] arrays inside the entries array
        entries.sort((a, b) => {
            if (a[1] === b[1]) {
                return a[0] < b[0] ? -1 : 1;
            }
            else {
                return b[1] - a[1];
            }
        })

        // convert count to percentages
        for (let entry of entries) {
	           entry[1] = entry[1] / this.totalLetters * 100;
        }

        // Drop any entries less than 1%
        entries = entries.filter(entry => entry[1] >= 1);
        // Now convert each entries to a line of text

        let lines = entries.map(([l, n]) => `${l}: ${"#".repeat(Math.round(n))} ${n.toFixed(2)}%`);
        // And return the concatenated lines, seperated by the newline char
        return lines.join("\n");
    }
}

// This async (promise-returning) function creates a Histogram object,
// asynchronously reads chunks of text from standard input, and add those
// chunks to histogram. When it reaches the end of stream, it returns the
// Histogram.

async function histogramFromStdin() {
    if (process.argv.length > 2) { // Check if command-line arguments are provided
        // If command-line arguments are present, concatenate them into a single string
        const inputText = process.argv.slice(2).join(" ");
        const histogram = new Histogram();
        histogram.add(inputText); // Add the input text to the histogram
        return histogram;

    } else {
        console.log("Please provide text via command-line arguments or standard input.");
        process.exit(1); // Exit the program if no input is provided
    }
}

  
// This final code is the main body of program
// It makes a Histogram object from standard input, then prints the Histogram
histogramFromStdin().then(histogram => {
    console.log(histogram.toString());
}).catch(error => {
    console.error("Error:", error); // Add error handling
});

console.log("Program finished (might print before histogram)"); // This might print before the histogram
```

```
// To run this program : 
node filename.js "Some text to analyze"
```

```
// The output
 : ################ 15.53%
E: ########## 10.30%
T: ######## 7.50%
S: ####### 6.81%
I: ####### 6.63%
N: ####### 6.63%
A: ##### 5.06%
O: #### 4.36%
R: #### 4.19%
L: #### 3.84%
M: ### 3.32%
P: ### 3.32%
U: ### 2.97%
D: ### 2.79%
H: ## 2.44%
Y: ## 2.27%
G: ## 1.92%
C: ## 1.75%
K: # 1.22%
F: # 1.05%
W: # 1.05%
```
