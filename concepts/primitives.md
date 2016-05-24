# Primitives

This section defines what the Material Motion Primitives are and how they relate
to one another.

Throughout the Starmap we use the term **element** to mean “an individual node
in a hierarchical layout/compositing engine”. This term is synonymous with a
View or a DOM Element.

This section’s topics:

- [Delta Primitives](#delta-primitives).

## Delta Primitives

Delta Primitives are built on the idea of change over time.

Time in a computer is not limited to “wall-clock” time - it can be slowed down,
stopped, or reversed. It can jump to arbitrary moments or it can be driven by
arbitrary systems. When time is mentioned throughout this document it means
“computer time”.

The following Delta Primitives are explored below:

- [Tweens](#tweens)
- Gesture recognition
- Physical simulation

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
