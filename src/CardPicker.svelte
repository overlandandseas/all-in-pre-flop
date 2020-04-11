<script>
  import { fade } from 'svelte/transition'
  import { createEventDispatcher } from 'svelte';

  export let cardObj;

  const SUIT_MAP = {
    spades: '♠',
    hearts: '♥',
    clubs: '♣',
    diamonds: '♦'
  }

  let isSelectionModalOpen = false;

  let suits = [
    { name: 'spades', icon: '♠', isRed: false },
    { name: 'hearts', icon: '♥', isRed: true },
    { name: 'clubs', icon: '♣', isRed: false },
    { name: 'diamonds', icon: '♦', isRed: true }
  ];

  let cards = ['A', 'K', 'Q', 'J', '10', '9', '8', '7', '6', '5', '4', '3', '2'];

  const dispatch = createEventDispatcher();

  function openSelectionModal() {
    isSelectionModalOpen = true;
  }

  function closeSelectionModal() {
    isSelectionModalOpen = false;
  }

  function handleKeyDownEvent(evt) {

    if(isSelectionModalOpen && evt.keyCode === 27) {
      closeSelectionModal()
    }
  }

  function noop (evt) {
    evt.stopPropagation()
  }

  function updateCardChosen(inCard) {
    console.dir(inCard);
    dispatch('updateCard', inCard);
    closeSelectionModal();
  }

  $: displayName = `${cardObj.value} ${SUIT_MAP[cardObj.suit]}`
</script>

<svelte:window on:keydown={handleKeyDownEvent}/>

<button class="w5 mv3 pa3 shadow-4 shadow-hover dim ba b--black-10" on:click={openSelectionModal}>
  <div class="tc">
    <div class="center w-75 h5 br3 bg-white ba b--black-30">
      <h1 class="ma2 code f-headline {cardObj.isRed ? 'red' : ''}">{displayName}</h1>
    </div>
  </div>
  <div class="tc">
    <h1 class="f4 black-70">Pick Card 2</h1>
  </div>
</button>


{#if isSelectionModalOpen}
  <section
    class="aspect-ratio--object bg-black-30"
    on:click={closeSelectionModal}
    transition:fade={{duration: 200}}>
    <div class="center mt3-l mw7-l ma0 bg-white pa3 ba b--black-10 shadow-1"
      on:click={noop}>
      <h1>Pick a card</h1>
      <table class="code w-100 tc f3 fw1" cellspacing="0">
        <thead>
          <tr>
            {#each suits as suit}
              <th class="fw6 bb b--black-20 pb3 pr3 {suit.isRed ? 'red' : ''}">{suit.icon}</th>
            {/each}
          </tr>
        </thead>
        <tbody class="lh-copy">
          {#each cards as card }
            <tr>
              {#each suits as suit}
                <td class="bb b--black-20">
                  <button
                    on:click={() => updateCardChosen({value: card, suit: suit.name, isRed: suit.isRed})}
                    class="underline-hover pv3 pr3 pointer bn bg-white  {suit.isRed ? 'red' : 'black-70'}">
                    {card}
                    </button>
                </td>
              {/each}
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  </section>
{/if}
