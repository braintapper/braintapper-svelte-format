<script>
  import Sugar from 'sugar-and-spice';

  if (typeof Date.create == "undefined") {
    // sugar was not already extended
    Sugar.extend();
  }
  // todo use a format string instead of css

  export let value = 0;
  export let showCurrency = true;
  export let showParentheses = true;
  // export let showCurrencyBefore = true;

  let containerClasses = []
  let amountClasses = []

  let formatAmount = (amount)=> {

    // reset the containerClasses
    containerClasses = []
    amountClasses = []

    if (amount != 0) {
      amount = amount / 100
    }

    if (showCurrency) {
      amountClasses.push("before-dollar");
    }

    if (amount < 0) {
      containerClasses.push( "negative")

      if (showParentheses) {
        containerClasses.push( "parentheses")
      }

    }
    return Math.abs(amount).format(2)

  }

  $: ledgerAmount = formatAmount(value);

</script>
<span class={containerClasses.join(" ")}><span class={amountClasses.join(" ")}>{ledgerAmount}</span></span>

<style>

.negative {
  color: var(--red); }

.parentheses:before {
  content: "("; }

.parentheses:after {
  content: ")"; }

.before-dollar:before {
  content: "$"; }

.after-dollar:after {
  content: "$"; }


</style>