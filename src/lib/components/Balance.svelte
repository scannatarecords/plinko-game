<script lang="ts">
  import { balance } from '$lib/stores/game';
  import { flyAndScale } from '$lib/utils/transitions';
  import { Popover } from 'bits-ui';

  // Reactive statement to format the balance whenever it changes
  $: balanceFormatted = $balance.toLocaleString('en-US', {
    minimumFractionDigits: 2,
    maximumFractionDigits: 2,
  });

  const addMoneyAmounts = [10000000, 500000000, 1000000000];
</script>


<div class="flex overflow-hidden rounded-md">
  <div
    class="flex gap-2 bg-slate-900 px-3 py-2 text-sm font-semibold text-white tabular-nums sm:text-base"
  >
    <span class="text-gray-500 select-none">$</span>
    <span class="min-w-16 text-right">
      {balanceFormatted}
    </span>
  </div>
  <Popover.Root>
    <Popover.Trigger
      class="bg-blue-600 px-4 py-2 text-sm font-medium text-white transition-colors hover:bg-blue-500 active:bg-blue-700 sm:text-base"
    >
      Add
    </Popover.Trigger>
    <Popover.Content
      forceMount
      sideOffset={8}
      class="z-30 max-w-lg space-y-2 rounded-md bg-slate-600 p-3"
    >
      {#snippet child({ wrapperProps, props, open })}
        {#if open}
          <div {...wrapperProps}>
            <div {...props} transition:flyAndScale>
              <p class="text-sm font-medium text-gray-200">Add money</p>
              <div class="flex gap-2">
                {#each addMoneyAmounts as amount}
                  <button
                    onclick={() => ($balance += amount)}
                    class="touch-manipulation rounded-md bg-green-500 px-3 py-2 text-sm font-semibold text-gray-900 transition-colors hover:bg-green-400 active:bg-green-600 disabled:bg-neutral-600 disabled:text-neutral-400"
                  >
                    +${amount}
                  </button>
                {/each}
              </div>
            </div>
          </div>
        {/if}
      {/snippet}
    </Popover.Content>
  </Popover.Root>
</div>
