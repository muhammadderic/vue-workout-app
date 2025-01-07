<script setup>
defineProps({
    handleChangeDisplay: Function
})

</script>

<template>
    <section id="welcome">
        <div class="benefits">
            <h2>Embark on this journey<br/>to achieve...</h2>

            <div>
                <p>✅ A straightforward regimen backed by real success stories.</p>
                <p>✅ Transform your body into its leanest, strongest version.</p>
                <p>✅ Gain expert knowledge about workouts and proper form.</p>
            </div>
        </div>

        <div>
            <h3>Our Fitness Blueprint</h3>
            <p>This program utilizes the time-tested <strong>Muscle Group Split</strong> approach.</p>
            <h5><i>Push &rarr; Pull &rarr; Legs &rarr; Repeat</i></h5>
        </div>

        <div class="card challenge">
            <h3>💪 Your Mission</h3>
            <p>Conquer every session and witness your transformation unfold!</p>
            <p>Are you ready to commit?</p>
            <button @click="() => handleChangeDisplay(2)">Begin &rarr;</button>
        </div>
    </section>
</template>

<style scoped>
    #welcome,
    .challenge,
    .benefits {
        display: flex;
        flex-direction: column;
    }

    #welcome {
        gap: 1.5rem;
    }

    .benefits {
        gap: 0.5rem;
    }

    .challenge {
        gap: 0.25rem;
    }

    @media (min-width: 640px) {
        #welcome {
            gap: 2rem;
            padding: 1rem 0;
        }

        .benefits {
            gap: 1rem;
        }
    }
</style>