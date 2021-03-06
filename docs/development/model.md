# Model

## Methods

Setting and getting attributes:

```js
// specific attribute
model.set('attributeName', value);

// multiple attributes
model.set({
    attributeName1: value1,
    attributeName2: value2,
});

// specific attribute
var value = model.get('attributeName');

// all attributes
var attributes = Espo.Utils.cloneDeep(model.attributes);

```

Saving model (to backend):

```js
// assuming model.id is set
model.save()
    .then(
        function () {

        }.bind(this)
    )
    .fail(
        function () {

        }.bind(this)
    );
```

Fetching model (from backend):

```js
// assuming model.id is set
model.fetch()
    .then(
        function () {

        }.bind(this)
    );
```

## Instantiating

Model-factory is available in views. The model-factory allows to create a model instance of a specific entity type.

```js
define('custom:views/some-custom-view', 'view', function (Dep) {
    return Dep.extend({
        setup: function () {
            var entityType = 'Account';
            // use wait to hold off rendering until model is loaded
            this.wait(
                this.getModelFactory().create(entityType)
                .then(
                    function (model) {
                        var entityType = model.entityType; // entityType property is set by the factory
                        this.model = model;
                        model.id = this.options.id;
                        return model.fetch(); // this will make API call using an appropriate URL
                    }.bind(this)
                )
                .then(
                    function () {
                        // here you can do some stuff with model
                    }.bind(this)
                )
            );
        },
    });
});
```

Instantiating w/o factory:

```js
define('custom:views/some-custom-view', ['view', 'model'], function (Dep, Model) {
    return Dep.extend({
        setup: function () {
            var model = new Model;
            model.urlRoot = 'MyModel'; // URL will be used when fetching and saving
            model.id = 'someId';            
            model.fetch(); // this will make GET MyModel/someId API call
        },
    });
});
```

## Events

Note: `listenTo` and `listenToOnce` are methods of *view*.

### change

When model attributes get changed (not necessarily synced with backend).

```js
this.listenTo(model, 'change', function (model, options) {
    if (this.model.hasChanged('someAttribute')) {
        // someAttribute is changed
    }
    if (options.ui) {
        // changed via UI
        // this options is set by field view
    }
}, this);

this.listenToOnce(model, 'change:someAttribute', function (model, value, options) {
    // someAttribute is changed
}, this);
```

### sync

Model synced with backend.

```js
this.listenTo(model, 'sync', function () {
    // synced with backend (fired after fetch or save)
}, this);
```

### destroy

Once model is removed (after *DELETE* request).

## Additional events

Defined in the application.

### after:relate

Once relationship panel updated. Available on detail/edit views.

### update-all

This event is not fired. But you can fire it to update all relationship panels. Available on detail/edit views.

## Other

Passing model to a child view:

```js
this.createView('someName', 'custom:views/some-view', {
    model: this.model,
});
```
