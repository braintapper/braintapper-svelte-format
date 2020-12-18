<script>

  import Sugar from 'sugar-and-spice';

  if (typeof Date.create == "undefined") {
    // sugar was not already extended
    Sugar.extend();
  }
  export let value = null;

  // todo: show time will change conditions

  

  let translate = (val, dummy) => {
    let theDate;

    if (value) {
      if (Object.isDate(val)) {
        theDate = val
      } else {
        theDate = Date.create(val).reset()
      }
      /// check if past or future
      /// if past - "ago"
      /// if future - "from now"

      let prefix;
      let suffix;
      let dateFormat = "%Y-%m-%d";
      let timeFormat = "%H:%M";

      isFuture = theDate.isFuture();

      if (isFuture) {
        prefix = "last"
        suffix = "from now"
      } else {
        prefix = "next"
        suffix = "ago"
      }

      isPast = !isFuture

      if (theDate.isToday() ) {
        return "Today"
      } else if (theDate.isTomorrow()) {
        return "Tomorrow"
      } else if (theDate.isYesterday()) {
        return "Yesterday"
      } else if (theDate.isBetween(Date.create().reset(), Date.create().reset().endOfWeek())) {
        return theDate.format("%A")
      } else if (theDate.isBetween(Date.create().reset().addWeeks(1).beginningOfWeek(), Date.create().reset().addWeeks(1).endOfWeek())) {
        return theDate.format("Next %A")
      } else {
        return theDate.format("%a, %b {d}, %Y")
      }
    } else {
      return "NULL"
    }

  }

  let dateDistance = (val, dummy) => {
    if (value) {
      let theDate;
      if (Object.isDate(val)) {
        theDate = val
      } else {
        theDate = Date.create(val).reset()
      }
      return Math.abs(theDate.daysSince(Date.create().reset()));
    } else {
      return 0
    }
  }




  let datePosition = (val, dummy) => {
    if (value) {
      if (Object.isDate(val)) {
        theDate = val;
      } else {
        theDate = Date.create(val).reset();
      }
      if (theDate.daysSince(Date.create().reset())  == 0) {
        return "present";
      } else if (theDate.daysSince(Date.create().reset())  < 0) {
        return "past";
      } else {
        return "future";
      }

    } else {
      return "present";
    }
  }


  let currentDate = Date.create().reset();

  let recalc = ()=> {
    currentDate = Date.create().reset();
  }

  recalc()

  recalc.every(60 * 1000) // recalc every minute

  $: output = translate(value, currentDate);
  $: distance = dateDistance(value, currentDate);
  $: direction = datePosition(value, currentDate);

</script>
{#if value == null}
NULL
{:else}
<div date="{currentDate}" {distance} {direction}>{output}</div>
{/if}

<style>
div[distance="0"],
div[distance="1"] {
  font-weight: bold; }

div[direction="past"] {
  color: var(--red); }

div[direction="present"] {
  color: var(--blue); }


</style>