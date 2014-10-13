property-store
===============

Simple utility for saving and restoring values of object properties.
It replaces verbose and meaningless "ifs" with shorter and more declarative syntax.

### Example usage ###
```
    
    var PropertyStore = require('property-store');


    function MyClass() {
        this._propertyStore = new PropertyStore(this);
        this._savePropertyValue = propertyStore.store.bind(propertyStore);
        this._restorePropertyValue = propertyStore.restore.bind(propertyStore);

        this.state = 'not ready';
    }


    MyClass.prototype.init = function() {
        this.state = 'ready';
    }

    MyClass.prototype.destroy = function() {
        this.state = 'not ready';

        // remove stored values and reference to instance
        this._propertyStore.destroy();
    }

    MyClass.prototype.doSomething = function() {
        if(this.state === 'ready') {
            // save current value of _state property
            this._savePropertyValue('state'); // or this._propertyStore.store('state');

            // set new value
            this.state = 'busy';

            setTimeout(function() {
                // get back to old value after finishing the job
                this._restorePropertyValue('state'); // or this._propertyStore.restore('state');
            }.bind(this), 1000);
        } else {
            throw new Error('object is not ready to perform the operation!');
        }
    }


    var myObj = new MyClass();

    myObj.init(); // state is 'ready'
    myObj.doSomething(); // state is 'busy'
    // after one second state is 'ready'

```