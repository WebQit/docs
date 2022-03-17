---
desc: Subscript Element.
---
# Subscript Element

## A Custom Element Example

As trivial as our hypothetical [`render()`](#whats-a-dependency-thread) function above is, we can see it applicable in real life places! Consider a neat reactive *Custom Element* example based on [`SubscriptElement`](https://webqit.io/tooling/oohtml/docs/spec/subscript#subscript-element-mixin) from [OOHTML](https://github.com/webqit/oohtml).

```js
// We'll still keep count as a global variable for now
let count = 10;
```

```js
// This custom element extends Subscript as a base class… more on this later
customElements.define( 'click-counter', class extends SubscriptElement( HTMLElement ) {
    
    // This is how we designate methods as reactive methods
    // But this is implicit having extended SubscriptElement()
    static get subscriptMethods() {
        return [ 'render' ];
    }
        
    connectedCallback() {
        // Full execution at connected time
        this.render();
        // Granularly-selective execution at click time
        this.addEventListener( 'click', () => {
            count ++;
            this.render.thread( [ 'count' ] );
        } );
    }

    render() {
        let countElement = document.querySelector( '#count' );
        countElement.innerHTML = count;
        
        let doubleCount = count * 2;
        let doubleCountElement = document.querySelector( '#double-count' );
        doubleCountElement.innerHTML = doubleCount;
        
        let quadCount = doubleCount * 2;
        let quadCountElement = document.querySelector( '#quad-count' );
        quadCountElement.innerHTML = quadCount;
    }
} );
```

*Continue to [SubscriptElement](https://webqit.io/tooling/oohtml/docs/spec/subscript#subscript-element-mixin) for the full story.*
