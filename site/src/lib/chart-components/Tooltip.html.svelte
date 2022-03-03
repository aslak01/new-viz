<!--
  @component
  Generates a hover tooltip. It creates a slot with an exposed variable via `let:detail` that contains information about the event. Use the slot to populate the body of the tooltip using the exposed variable `detail`.
 -->
<script>
  /** @type {Object} evt - A svelte event created via [`dispatch`](https://svelte.dev/docs#createEventDispatcher) with event information under `evt.detail.e`. */
  export let evt = {}

  /** @type {Number} [offset=-35] - A y-offset from the hover point, in pixels. */
  export let offset = -0

  $: elWidth =
    evt.detail.e.srcElement.farthestViewportElement.getBoundingClientRect()
      .width
  $: xPos = evt.detail.e.layerX
  $: reverse = xPos > elWidth - 300 ? xPos - 230 : xPos
  $: reverseText = xPos > elWidth - 300 ? true : false
</script>

{#if evt.detail}
  <div
    class={'tooltip' + (reverseText ? ' tooltip-right' : '')}
    style="
      top:{evt.detail.e.layerY + offset}px;
      left:{reverse}px;
    "
  >
    <slot detail={evt.detail} />
  </div>
{/if}

<style>
  .tooltip {
    position: absolute;
    font-weight: bold;
    width: 200px;
    color: white;
    font-size: 13px;
    text-shadow: 1px 1px 5px black;
    /* background: rgba(0, 0, 0, 1); */
    transform: translate(-30px, -60%);
    padding: 5px;
    z-index: 15;
  }
  .tooltip-right {
    text-align: right;
  }
</style>
