<script lang="ts">
    import { Select } from "bits-ui";
    import type { EventHandler } from "svelte/elements";

    type Choice = { label: string; value: string };

    let {
        choices,
        placeholder,
        value = $bindable(),
        open = $bindable(false),
        onselect,
    }: {
        choices: Choice[];
        placeholder: string;
        value?: string;
        open?: boolean;
        onselect?: EventHandler<Event, HTMLSelectElement> | null | undefined;
    } = $props();

    const selectedLabel = $derived(
        value
            ? choices.find((choice: any) => choice.value === value)?.label
            : placeholder,
    );
</script>

<Select.Root
    type="single"
    onValueChange={(v) => (value = v)}
    items={choices}
    bind:open
>
    <Select.Trigger class="select-trigger" aria-label={placeholder}>
        <span class="select-trigger-text" class:has-value={value !== undefined}
            >{selectedLabel}</span
        >
    </Select.Trigger>
    <Select.Portal>
        <Select.Content class="select-content dialog" sideOffset={8}>
            <Select.Viewport class="select-viewport">
                {#each choices as choice, i (i + choice.value)}
                    <Select.Item
                        class="select-item"
                        value={choice.value}
                        label={choice.label}
                    >
                        {#snippet children({ selected })}
                            {choice.label}
                        {/snippet}
                    </Select.Item>
                {/each}
            </Select.Viewport>
        </Select.Content>
    </Select.Portal>
</Select.Root>

<style>
    :global(.scroll-wrapper) {
        display: flex;
        width: 100%;
        align-items: center;
        justify-content: center;
    }

    :global(.select-trigger) {
        position: relative;
        width: 100%;
        height: 2.5rem;
        padding: 0.5rem;
        padding-left: 1rem;
        display: inline-flex;

        border-radius: 0.5rem;
        overflow: hidden;
        align-items: center;
        background-color: var(--dark-3);
        font-size: 1rem;
        user-select: none;
        gap: 0.5rem;
        padding-right: 0.5rem;
        cursor: pointer;

        :global(.select-item) {
            color: var(--light-1);

            &:disabled {
                color: var(--light-3);
            }
        }

        --icon-stroke: var(--light-3);

        &.has-icon-left {
            padding-left: 0.5rem;
        }
    }

    :global(.select-trigger-text) {
        width: 100%;
        text-align: start;
        color: var(--light-3);
        &.has-value {
            color: var(--light-1);
        }
    }

    :global(.select-content) {
        z-index: 50;
        padding: 0.25rem;
        border-radius: 0.75rem;
        border-width: 1px;
        outline-style: hidden;
        max-height: 24rem;
        user-select: none;
        max-height: var(--bits-select-content-available-height);
        width: var(--bits-select-anchor-width);
        gap: 0;
    }

    :global(.select-viewport) {
        padding: 0.25rem;
        width: 100%;
        overflow-y: auto;
    }

    :global(.select-item) {
        display: flex;
        align-items: center;
        width: 100%;
        height: 2.5rem;
        padding: 0.25rem 0.5rem;
        user-select: none;
        border-radius: 0.5rem;
        color: var(--light-2);
        cursor: pointer;

        &:hover {
            color: var(--light-1);
            background-color: var(--dark-2);
        }
    }
</style>
