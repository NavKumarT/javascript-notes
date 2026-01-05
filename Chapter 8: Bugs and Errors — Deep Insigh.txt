Chapter 8: Bugs and Errors — Deep Insights & Interview Notes
This chapter addresses the inevitability of flaws in programs and the mechanisms JavaScript provides to find, diagnose, and handle them.

1. The Nature of Bugs and JavaScript’s Looseness
• Categorization: Bugs generally fall into two categories: those caused by confused thoughts and those caused by mistakes in converting thoughts to code.
• The Silence Gotcha: Because JavaScript is "loose," many nonsensical computations (like true * "monkey") do not trigger immediate errors. Instead, they quietly produce NaN or undefined, allowing the program to continue with bogus values that only cause visible failures much later, making the source hard to track.
• Debugging Workflow: Effective debugging is a cycle of analyzing behavior, forming a theory, and making observations (via console.log or breakpoints) to test that theory.

2. Strict Mode ("use strict")
• Activation: Placing "use strict" at the top of a file or function makes the language less liberal. Note that classes and modules enable strict mode automatically.
• Key Protections:
    ◦ Accidental Globals: In normal mode, forgetting let creates a global binding; in strict mode, it throws a ReferenceError.
    ◦ The this Binding: In strict mode, this is undefined in functions not called as methods. Outside strict mode, it refers to the global scope object, which can lead to constructors accidentally overwriting global variables if called without new.
• Feature Removal: Strict mode removes problematic features like the with statement.

3. Error Propagation Strategies
Interviews often focus on when to use Special Values vs. Exceptions.
• Special Values: Functions can return null, undefined, or an object like {failed: true} to indicate error. This is best when errors are common and expected, forcing the caller to explicitly handle them.
• The Downsides: Returning special values can lead to awkward code if every caller must repeatedly check for null before proceeding.
• Exceptions: Used when a function cannot proceed normally and needs to immediately jump to a handler. This allows the "intermediate" functions in the call stack to remain clean of error-handling logic.

4. Deep Exception Mechanics
• Unwinding the Stack: When an exception is thrown, it "zooms down" the call stack, discarding all contexts until it hits a catch block.
• The Error Object: Using throw new Error("message") is standard because it captures a stack trace, showing exactly where the failure occurred.
• The finally Block: This block runs no matter what—whether the try block succeeds or an exception is caught. It is critical for cleaning up side effects or repairing inconsistent state.
• State Consistency Insight: A programming style that computes new values (functional/persistent) instead of changing existing data is more resilient to exceptions because it doesn't leave "broken" data structures behind if a process is interrupted.

5. Selective Catching: The custom Error Pattern
• The Blanket-Catch Gotcha: Catching everything with a simple catch (e) {} is dangerous because it buries bugs like typos or ReferenceErrors that you didn't intend to catch.
• Custom Errors: To catch only specific errors (like bad user input), define a class that extends Error.
• The instanceof Check: Use instanceof inside the catch block to identify your custom error. If the error is not the one you expect, rethrow it so it can be handled elsewhere or crash the program appropriately.

6. Assertions
• Definition: Assertions are checks intended to find programmer mistakes, not to handle normal operation errors.
• Example: Throwing an error if a function that requires a non-empty array is called with []. This ensures the program "blows up" loudly at the point of misuse rather than failing silently later.

--------------------------------------------------------------------------------
Analogy for Exception Handling: Think of a function call stack like a relay race. Error propagation is like a runner realizing they dropped the baton and stopping the whole race to tell the coach. Exceptions are like the runner immediately shouting for a medic; the medic (catch) might be at the finish line or along the track, but until the medic is found, all other runners in that specific heat stop what they are doing.