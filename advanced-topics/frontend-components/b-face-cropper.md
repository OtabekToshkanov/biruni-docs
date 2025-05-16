---
description: >-
  Face-cropper is used to crop user profile photos. Face-cropper can be used
  with face-shape in order to crop profile photos on the same scale.
icon: face-viewfinder
---

# b-face-cropper

## Face cropper modal can be called in any form as below:

```javascript
page.faceCropper($file, onCrop, rounded, maskEnabled, maskMode);
```

## Parameter descriptions:

<table><thead><tr><th width="134">parameter</th><th width="136" align="center">is required</th><th width="159" align="center">default</th><th width="116" align="center">type</th><th>description</th></tr></thead><tbody><tr><td><code>$file</code></td><td align="center">required</td><td align="center">-</td><td align="center">File(), Object</td><td>input image file, or object of photo with keys: SHA, name, type</td></tr><tr><td><code>onCrop</code></td><td align="center">required</td><td align="center">-</td><td align="center">Function</td><td>Callback function which gets cropped image file as a parameter</td></tr><tr><td><code>rounded</code></td><td align="center">optional</td><td align="center">false</td><td align="center">Boolean</td><td>true - rounded cropbox, false - square cropbox</td></tr><tr><td><code>maskEnabled</code></td><td align="center">optional</td><td align="center">false</td><td align="center">Boolean</td><td><p>true - enables masked cropbox,</p><p>false - simple cropbox</p></td></tr><tr><td><code>maskMode</code></td><td align="center">optional</td><td align="center">if maskEnabled true then 1, else 0</td><td align="center">Number</td><td><p>0 - no mask,</p><p>1 - face mask,</p><p>2 - grid overlay</p></td></tr></tbody></table>

## Example with face shape:

```javascript
function openCropper($file) {
  page.faceCropper($file, croppedPhoto => {
    d.photo = croppedPhoto;
  }, true, true);
}
```

<figure><img src="../../.gitbook/assets/advanced-topics/frontend-componenets/face-mask.png" alt=""><figcaption></figcaption></figure>

## Example without face shape:

```javascript
function openCropper($file) {
  page.faceCropper($file, croppedPhoto => {
    d.photo = croppedPhoto;
  }, true);
}
```

<figure><img src="../../.gitbook/assets/advanced-topics/frontend-componenets/face-round.png" alt=""><figcaption></figcaption></figure>

## Example with grid overlay:

```javascript
function openCropper($file) {
  page.faceCropper($file, croppedPhoto => {
    d.photo = croppedPhoto;
  }, true, true, 2);
}
```

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## Hotketys:

| hotkey    | action                                        |
| --------- | --------------------------------------------- |
| `escape`  | closes the cropper modal                      |
| `enter`   | crops & saves the photo, and closes the modal |
| `↑`       | moves crop-box up                             |
| `↓`       | moves crop-box down                           |
| `←`       | moves crop-box left                           |
| `→`       | moves crop-box right                          |
| `alt + ↑` | moves crop-box up faster                      |
| `alt + ↓` | moves crop-box down faster                    |
| `alt + ←` | moves crop-box left faster                    |
| `alt + →` | moves crop-box right faster                   |
| `[`       | zooms out crop-box                            |
| `]`       | zooms in crop-box                             |
| `alt + [` | zooms out crop-box faster                     |
| `alt + ]` | zooms in crop-box faster                      |
