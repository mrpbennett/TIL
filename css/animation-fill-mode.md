# animation-fill-mode

The `animation-fill-mode` property specifies a style for the element when the animation is not playing (before it starts, after it ends, or both).

CSS animations do not affect the element before the first keyframe is played or after the last keyframe is played. The `animation-fill-mode` property can override this behavior.

### Example:

When using keyframes on a button and it's hover state. You don't want the hover color to revert back to the oringal one.

```css
button:hover {
    animation-name: blue;
    animation-duration: 500ms;
    animation-fill-mode: forwards;
}
@keyframes background-color {
    100% {
        background-color: #4791d0;
    }
}
```
