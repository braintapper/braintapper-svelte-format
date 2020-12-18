<script>

  
  // TODO: padding right
  // TODO: popover on mouseover when ellipsis == true

  export let style = undefined;
  export let offset = 0;
  export let ellipsisActive = false;

  let parentWidth = undefined;
  let childWidth = undefined;
  
  $: ellipsisActive = ((childWidth + parseInt(offset)) > parentWidth)

</script>


<div container {style} ellipsis={ellipsisActive} {childWidth} {parentWidth} bind:clientWidth={parentWidth}>
  <span subcontainer bind:offsetWidth={childWidth}>
    <slot/>
  </span>
</div>

<style>
  [container] {
    max-width: calc(100% - 1px);
    width: calc(100% - 1px);
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    display: block; }

  [subcontainer] {
    max-width: calc(100% - 1px);
    width: calc(100% - 1px); }
</style>