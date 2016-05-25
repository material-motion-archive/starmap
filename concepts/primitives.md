# Primitives

This section defines what the Material Motion Primitives are and how they relate
to one another.

Throughout the Starmap we use the term **element** to mean “an individual node
in a hierarchical layout/compositing engine”. This term is synonymous with a
View or a DOM Element.

This section’s topics:

- [Delta Primitives](#delta-primitives),
- the [Timeline](#timeline).

## Delta Primitives

Delta Primitives are built on the idea of change over time.

Time in a computer is not limited to “wall-clock” time - it can be slowed down,
stopped, or reversed. It can jump to arbitrary moments or it can be driven by
arbitrary systems. When time is mentioned throughout this document it means
“computer time”.

The following Delta Primitives are explored below:

- [Tweens](#tweens)
- [Gesture recognition](#gesture-recognition)
- [Physical simulation](#physical-simulation)

Please note that these Primitives can be applied to an arbitrary number of
dimensions and types of input.

### Tweens

**What it is**: *interpolated change between a list of values*.

Tweens have a **starting time** and a **duration**. The start and duration
properties allow Tweens to be sequenced in relation to other tweens.

Tweens use an **interpolation function**. This is generally a cubic-bezier.

A **keyframe animation** is a Tween that animates between two or more values.

| Platform | Keyframe |
|:--------:|:--------:|
| Android | [Property Animation](http://developer.android.com/guide/topics/graphics/prop-animation.html) |
| iOS | [CAKeyframeAnimation](https://developer.apple.com/library/mac/documentation/GraphicsImaging/Reference/CAKeyframeAnimation_class/) |
| Web CSS | [transition](https://developer.mozilla.org/en-US/docs/Web/CSS/transition)[@keyframes](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes) |
| Web JavaScript | [element.animate](https://github.com/web-animations/web-animations-js/) |

### Gesture recognition

**What it is**: *recognition of continuous or discrete actions*.

**Registration**: Gesture recognizers can be associated with a specific
element. Once associated, interactions with the given element will be
interpreted by the gesture recognizer. Interpreted values will be generated
when the gesture is recognized. Gestures may be recognized continuously (many
times) or discretely (once).

**Simultaneous gesture recognition**: Multiple gesture recognizers can be
associated with a single element. All associated gesture recognizers should be
capable of generating values simultaneously. For instance:

> Two pan gestures are registered to a carousel:
> 
> - horizontal pans move between items in the carousel
> - vertical pans collapse or expand the carousel.
> 
> Both gestures can occur simultaneously.

**Gesture dependencies**: Gesture recognizers can defer recognition until other
recognizers have failed. For instance:

> An element can both be tapped and double-tapped; tap is deferred until the
> failure of double-tap.

**Velocity**: Continuous gesture recognizers emit a velocity each time the
gesture generates a new interpreted value. Once a gesture has ended, its
velocity may be fed into a Physical Simulation.

| Platform | Pan | Pinch | Rotate | Tap |
|:--------:|:---:|:-----:|:------:|:---:|
| Android | [ViewDragHelper](https://developer.android.com/reference/android/support/v4/widget/ViewDragHelper.html) / [custom](http://developer.android.com/training/gestures/scale.html#drag) | [ScaleGestureDetector](http://developer.android.com/training/gestures/scale.html#scale) / custom | custom | [OnClickListener](http://developer.android.com/reference/android/view/View.OnClickListener.html) / [GestureDetector](http://developer.android.com/training/gestures/detector.html#detect) |
| iOS | [UIPanGestureRecognizer](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIPanGestureRecognizer_Class/) | [UIPinchGestureRecognizer](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIPinchGestureRecognizer_Class/) | [UIRotateGestureRecognizer](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIRotateGestureRecognizer_Class/) | [UITapGestureRecognizer](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITapGestureRecognizer_Class/) |
| Web | [hammer.js](http://hammerjs.github.io/) | | | |

### Physical simulation

**What it is**: *the application of physical forces to a simulated body
consisting of both a position and velocity*.

Forces can be applied to the physical body’s velocity over time using a
numerical integrator
([RK4](https://en.wikipedia.org/wiki/Runge%E2%80%93Kutta_methods) is one such
integrator).

**Common forces**: The most common types of forces for software interfaces are:

- [Damped harmonic oscillators](https://en.wikipedia.org/wiki/Harmonic_oscillator#Damped_harmonic_oscillator) (Springs)
- Laminar [drag](https://en.wikipedia.org/wiki/Drag_(physics)) (Friction)
- [Collisions](https://en.wikipedia.org/wiki/Collision_detection)

**Custom forces**: A physical simulation system should also allow for the
expression of arbitrary forces.

| Platform | Springs | Friction | Collisions | Custom |
|:--------:|:-------:|:--------:|:----------:|:------:|
| Android | [Rebound](https://github.com/facebook/rebound) | | | |
| iOS | [UIAttachmentBehavior](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/) · [POP](https://github.com/facebook/pop) | [UIFieldBehavior.dragField](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIFieldBehavior_class/) | [UICollisionBehavior](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollisionBehavior_Class/) | [UIAttachmentBehavior](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDynamicBehavior_Class/) · [POP](https://github.com/facebook/pop) |
| Web JavaScript | [Rebound.js](https://github.com/facebook/rebound-js/) | | | |
| Web React | [React Motion](https://github.com/chenglou/react-motion/) | | | |

## Timeline

**What it is**: *an entity that has a floating point value ranging from 0 to 1 inclusively that can
be driven by a Delta Primitive and to which Tweens may be associated*.

A Timeline has a **progress** that is a floating point number. The Timeline bounds the progress
between 0 and 1.

We use “**to drive**” to refer to the idea of *an output from one Primitive being fed into the input
of another*. This enables the expression of novel interactions such as a Gesture driving a Timeline
that is driving a collection of Tweens.

#### Primitives driving the progress

The **progress** can be driven by the following Delta Primitives if they have been mapped to the
[0...1] domain:

- **Time** can move the progress forward or backward.
- **Gestures** can scrub the progress directly.
- **Physical simulation**: The progress can be physically anchored to a position, usually 1 or 0,
  using a Spring. A Gesture primitive’s final velocity can also be fed into this simulation.
- **Tweens** can drive the progress.

#### Progress driving Primitives

The progress can drive Tween Primitives. For example: a FadeIn tween animation could occur during
the first 50% of a timeline.

There is no known benefit to driving Gestural or Physical Simulation Primitives with a Timeline; it
is also not particularly clear what that would mean.
