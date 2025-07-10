2025-07-090952
Tags: #
__
```
mixin UpdateMarkingMixin {

bool _needsUpdate = false;
final List<Function> _callbacks = [];

void setUpdateCallbacks(List<Function> callbacks) => _callbacks.addAll(callbacks);
void markNeedsUpdate() => _needsUpdate = true;
void updateIfNecessary() {
if (_needsUpdate) {
_needsUpdate = false;
for (final callback in _callbacks) {
callback();}}}
}

```



__
###Zero-Links
-
__
###Links
-