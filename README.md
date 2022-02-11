<img width="300" alt="image" src="https://user-images.githubusercontent.com/35671734/153657813-20409815-9d24-468e-a16c-fa990e6eecbd.png">


# Svelte Smooth Corners
Svelte component to create a wrapping element with smooth corners rather then relying on radial corners – best practices in design.

To my current knowledge all current browser versions support this package.

## Installation
```
npm install svelte-smooth-corners
```

## Usage 
```svelte
<script>
  import SmoothCorners from "svelte-smooth-corners";
</script>

<SmoothCorners cornerRadius="10" cornerSharpness="9">
  Hello Squircle World!
</SmoothCorners>
```


## Examples
### »I need borders, pls help!«
```svelte
<script>
  import SmoothCorners from "svelte-smooth-corners";
</script>

<style>
.border {
  background: green;
  padding: 5px;
}
</style>

<SmoothCorners cornerRadius="10" cornerSharpness="9">
  <div class="border">
    <SmoothCorners cornerRadius="5" cornerSharpness="9">
      Hello Squircle World!
    </SmoothCorners>
  </div>
</SmoothCorners>
```

<img width="544" alt="image" src="https://user-images.githubusercontent.com/35671734/153605412-e276d899-fcf5-41de-92ef-742b7f1ec867.png">


### Use the clip path only
In case you need the option to add css to your elements _and_ need the smooth corners you can use the clip path only mode.
For that, add `clipPathOnly={true}` to the `<SmoothCorners>` component. Then you can access the clip path SVG name via the bound `name`-prop of the component. Add this SVG path as CSS var to the element and use it as `clip-path` value.
```svelte
<script>
  import SmoothCorners from "svelte-smooth-corners";
  
  let w, h, clipPathName
</script>

<style>
.foo {
  clip-path: var(--clipPath);
  -webkit-clip-path: var(--clipPath);
}
</style>

<SmoothCorners clipPathOnly={true} w={w} h={h} bind:name={clipPathName} cornerRadius="10" cornerSharpness="9"></SmoothCorners>
<div class="foo" style="--clipPath: url('#{clipPathName}')">
  Hello Squircle World!
</div>
```
