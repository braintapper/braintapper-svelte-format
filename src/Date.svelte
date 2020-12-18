<script>
  import Sugar from 'sugar-and-spice';

  if (typeof Date.create == "undefined") {
    // sugar was not already extended
    Sugar.extend();
  }

  export let value = undefined;
  export let fromId = false;

  let render = (val) => {
    if (val) {
      if (fromId) {
        return Date.create(`${val}`.insert("-",6).insert("-",4));
      } else {
        return Date.create(val).format();
      }
    } else {
      return ""
    }
  }

  $: output = render(value);

</script>
{output}
