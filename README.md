# Smooth corners
Svelte component to create a wrapping element with smooth corners rather then relying on radial corners – best practices in design.

To my current knowledge all current browser versions support this package.

## Installation
```
npm install gustavneustadt/svelte-smooth-corners
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


## I need borders, pls help!
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
