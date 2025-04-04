<script lang="ts">
    import Select from "$lib/Select.svelte";

    let selectValue: string[] = $state([]);

    const selectChoices = [
        { value: "light-monochrome", label: "Light Monochrome" },
        { value: "dark-green", label: "Dark Green" },
        { value: "svelte-orange", label: "Svelte Orange" },
        { value: "punk-pink", label: "Punk Pink" },
        { value: "ocean-blue", label: "Ocean Blue", disabled: true },
        { value: "sunset-orange", label: "Sunset Orange" },
    ];
</script>

<section class="column">
    <Select choices={selectChoices} placeholder="Select a Theme"></Select>
</section>
