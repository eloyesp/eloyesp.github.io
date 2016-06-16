---
title: Has many Inputs
---

Once again I'm writing nested forms with have many relations, so I have to write again that code that handle _"add another field"_.

This is a recurrent issue I've solved it many times, the solution is always ugly so the next time I start it again. The others solutions I find, like [cocoon][1] or [nested_form][2] are too complex.

Now I've made something I like (I didn't make the package yet so I have no code to link), it uses the `<template>` tag (from html5).

The code is something like:

```javascript
$(document).on('click', 'button.add-item', function () {
  var button = $(this)
  var replaceUUID = (function () {
    var uuid = Math.floor(Math.random() * (1e18))
    return function (i, val) {
      if (typeof val !== "undefined" && val !== null) {
        return val.replace(/UUID/i, uuid)
      }
    }
  })()
  $(button.next('template')[0].content)
    .children()
    .clone()
    .find('label,input')
      .attr('for', replaceUUID)
      .attr('id', replaceUUID)
      .attr('name', replaceUUID)
      .end()
    .appendTo(button.closest('ul'))
})
```

And the markup is rather simple too:

```erb
<ul class="list-group">
  <%= form.fields_for :items do |f| %>
    <li class="list-group-item">
      <%= f.input :title %>
    </li>
  <% end %>
  <li class="list-group-item">
    <button type="button" class="btn btn-default add-item">
      "Add item"
    </button>
    <template>
      <li class="list-group-item">
        <%= form.fields_for :items, Item.new, child_index: 'UUID' do |f| %>
          <%= f.input :title %>
        <% end %>
      </li>
    </template>
  </li>
</ul>
```
 
The code is not final (I don't need jQuery) and I have to do some cleanup, but it's rather straightforward, when you click the button, it clone the template and replace the UUID with an actual random number. The only magic part, is the [`child_index`][3] that is not documented.

 [1]: https://github.com/nathanvda/cocoon
 [2]: https://github.com/ryanb/nested_form
 [3]: https://github.com/rails/rails/blob/27cc32f43e62ee98a8bbcba9ac1b97233293dc26/actionview/lib/action_view/helpers/form_helper.rb#L1345
