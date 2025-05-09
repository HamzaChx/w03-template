<script lang="ts">
    import { onMount } from "svelte";
    import "../app.css";
    import Icon from "@iconify/svelte";
    import { env } from '$env/dynamic/public';
    import FoodCard from './FoodCard.svelte';
    import type { Meal } from '$lib/types';

    // Use environment variable for API URL with fallback to localhost
    let baseUrl = env.PUBLIC_API_URL || "http://localhost:8090";

    // For more information on runes and reactivity, see: https://svelte.dev/docs/svelte/what-are-runes
    let meals: Meal[] = $state([]);

    // Fetch data once on component mount
    onMount(async () => {
         const response = await fetch(`${baseUrl}/mensa-garching/today`);
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            meals = await response.json();
            console.log(meals);
    });
</script>

<main>
    <header>
        <h1>Garching Campus Canteen</h1>
        <p>Today's menu offerings</p>
    </header>

    {#if meals.length === 0}
        <div class="no-results">
            <p>Loading menu items...</p>
        </div>
    {:else}
        <div class="food-grid">
            {#each meals as meal}
                <FoodCard {meal} />
            {/each}
        </div>
    {/if}

    {#if meals.length === 0 && meals.length > 0}
        <div class="no-results">
            No menu items match your filters. Try changing your selection.
        </div>
    {/if}
</main>
