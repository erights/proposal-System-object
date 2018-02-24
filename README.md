# proposal-System-object
a vettable System object for standard power-granters

## Abstract

We propose a new standard object named `System` to hold all new capabilities introduced by the JavaScript language that grant special powers---such as `makeWeakRef`, `getStack`, `defaultLoader`---that should be denied to code of "normal" priviledge. We place `System` in the global lexical scope. As a result, it is not reachable from the global object except through the *unsafe-evaluators*. In an environment in which *unsafe-evaluators* are suppressed, such as by CSP `unsafe-eval` controls, the `System` object can only be introduced into a program by code that explicitly mentions it as a lexical variable name.

## The old threat model

Due to a historic accident, JavaScript has an almost perfect distinction analogous to the hardware/OS distinction between *user-mode* and *system-mode* instructions. 

The *primordials* are those objects, other than the global object, that are mandated by the EcmaScript standard to exist in a Realm before any code starts running. As of ES2017, the JavaScript primordials contain no hidden state, I/O abilities, or action-at-a-distance that violates ocap rules. (The exception are `Date.now`, the `Date` constructor when called with zero arguments, and `Math.random`, about which more below.) In an *ocap environment* in which all the primordials are frozen and the evaluators have been tamed---such as Caja, SES, the Locker Service, and Frozen Realms---object subgraphs that share only a common set of primordials cannot communicate with each other. 

There are now several proposals which would introduce special powers, analogous to *system-mode-like* powers. 
   * WeakReferences make garbage collection observable, and opens a new side channel that other's can signal merely by dropping a reference to an otherwise-powerless immutable object.
   * Error stacks make visible a traceback of the stack captured by an Error object on creation.
   * A loader instance can be use to cause I/O to the external world.
   
These ocap environments need to censor or virtualize each of these. Over time, we can expect more such special powers to be introduced to the standard. TODO more

## A new threat model

At (TODO) Mike Samuel explains an interesting new threat model. Summarizing, imagine that a small number of security experts seeks to safely vet the code of a large number of non-experts who make various forms of accident. In order to economically vet with regard to certain threats, the experts would like to look for red flags in the code where relevant hazards may be introduced. The red flags need to typically be rare or no effort is saved. But this strategy is only safe if these particular hazards cannot be introduced by code without these red flags.

## 
