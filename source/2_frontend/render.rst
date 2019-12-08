页面渲染
===============

随着构式类型的增多，标注菜单不断扩展，我们需要针对不同的构式实例，渲染不同的参数信息，如此一来，``Blade`` 便有了\
缺陷，其不能循环输出参数，因此，在 ``/resources/views/edit_pages/editor.blade.php`` 中：

.. code-block:: html

   <p class="sentence" id="sentence-{{ $count++ }}" data-annotated="{{ $sentence['annotated']  }}">
       @foreach ($sentence['nodes'] as $node)
           @if (key($node) == "#text")
               {{ current($node) }}
           @else
               @foreach (current($node)['attributes'] as $attribute)
                   <span data-attribute="data-cxn-{{ key($attribute) }}" style="display:none">{{ current($attribute) }}</span>
               @endforeach
               <span class="{{ key($node) }}">
               @foreach (current($node)["components"] as $component)
                   @if (key($component) == '#text')
                       {{ current($component) }}
                   @else
                       <span class="{{ key($component) }}">{{ current($component) }}</span>
                   @endif
               @endforeach
               </span>
           @endif
       @endforeach
   </p>

上述代码首先渲染句子中非构式成分，对于构式成分，将其参数渲染为一组不可见的 ``<span>`` ，再渲染构式成分。而后，\
再将这组不可见的 ``<span>`` 的值设定为构式成分的参数，并最终删除这组不可见的 ``<span>`` 。

.. code-block:: JavaScript

   // editor.js
   let sentences = $('p.sentence');
   $.each(sentences, (index, element) => {
       let attributes = {};
       if (element.children.length != 0) {
           for (let i = 0; i < element.children.length; i++) {
               if (element.children[i].getAttribute('data-attribute')) {
                   let key = element.children[i].getAttribute('data-attribute'),
                       value = element.children[i].textContent;
                   attributes[key] = value;
               }
           }

           for (let i = 0; i < element.children.length; i++) {
               if (element.children[i].className == 'cxn') {
                   for (const key of Object.keys(attributes)) {
                       element.children[i].setAttribute(key, attributes[key]);
                   }
               }
           }
       }
   });
   $('span[data-attribute]').remove();

