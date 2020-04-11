<script>
  import CardPicker from './CardPicker.svelte'


  let card1 = {
    value: 'A',
    suit: 'spades',
    isRed: false
  }

  let card2 = {
    value: 'A',
    suit: 'hearts',
    isRed: true
  }


  $: odds = updateOdds(card1, card2);

  function updateOdds(card1, card2) {
    // do whatever to get the odds
    // && is to avoid compiling changes away
    return card1 && card2 && (Math.random() * 15).toFixed(2);
  }

</script>
<div class="w-100 bt b--dark-red bw2">
<main class="mw7-l center-l mh3">
  <h1>All in pre-flop calculator</h1>
  <section class="flex justify-between items-end" >
    <CardPicker
      on:updateCard={card => card1 = card.detail}
      cardObj={card1}
    />
    <article>
      <h3 class="f3">Odds: <span class="code">{odds}%</span></h3>
    </article>
    <CardPicker
      on:updateCard={card => card2 = card.detail}
      cardObj={card2}
    />
  </section>

</main>
</div>
