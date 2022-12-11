<template>
    <div id="racine" class="racine">
        <a href="#racine" class="go-up">v</a>
        <div class="only-mobile">Sorry only mobile 300px width</div>
        <div class="header">
            <h1>Tournée TL13</h1>
            <input
                type="search"
                v-model="rueBox"
                class="search-box"
                placeholder="Chercher une rue"
            />
        </div>
        <div class="rues">
            <div v-if="filteredRuesNumber.length === 0">Pas de resultat(s)</div>
            <div
                v-for="rue in filteredRuesNumber"
                :key="rue"
                class="rue"
                :style="{ 'background-color': rue.bgColor }"
            >
                <!-- Rue name -->
                <h3 class="rue-name">
                    <span class="rue-number">
                        {{ rue.number }}
                    </span>
                </h3>
                <!-- Rue options -->
                <div class="optins-courrier">
                    <label v-for="option in rue.options" :key="option">
                        <input
                            class="option"
                            type="checkbox"
                            :data-name="option + rue.number"
                            @change="setOptionPersistance"
                        />
                        <span> {{ option }} </span>
                    </label>
                </div>
            </div>
        </div>

        <!-- Fin de tournée -->
        <hr />
        <div class="fin" v-if="filteredRuesNumber.length > 0">
            <button @click="clearTournee">Fin de tournée</button>
        </div>
    </div>
</template>

<script>
import rues from "./JSON";
import { onMounted, computed, ref } from "vue";
//
export default {
    setup() {
        const rueBox = ref("");
        //
        onMounted(function name() {
            const options = document.querySelectorAll(".option");
            options.forEach(function (option) {
                const optionDataName = option.dataset.name;
                const target_ID = localStorage.getItem(optionDataName);
                if (target_ID) {
                    option.checked = true;
                }
            });
            //
            window.addEventListener("scroll", function () {
                if (window.pageYOffset > 1000) {
                    document.querySelector(".go-up").style.opacity = 1;
                } else {
                    document.querySelector(".go-up").style.opacity = 0;
                }
            });
        });
        //
        function setOptionPersistance(event) {
            const target = event.target;
            const target_ID = target.dataset.name;
            localStorage.setItem(target_ID, target_ID);
            if (target.checked === false) {
                localStorage.removeItem(target_ID);
            }
        }
        //
        const filteredRuesNumber = computed(function () {
            return rues.filter(function (rue) {
                return rue.number
                    .toLowerCase()
                    .includes(rueBox.value.toLowerCase());
            });
        });
        //
        function clearTournee() {
            const options = document.querySelectorAll(".option");
            options.forEach(function (option) {
                const optionDataName = option.dataset.name;
                const target_ID = localStorage.getItem(optionDataName);
                if (target_ID) {
                    localStorage.removeItem(target_ID);
                    location.replace("#racine");
                } else {
                    location.reload();
                }
            });
        }
        //
        return {
            rues,
            rueBox,
            clearTournee,
            filteredRuesNumber,
            setOptionPersistance,
        };
    },
};
</script>

<style lang="scss">
/* Only Mobile */

@media (max-width: 300px) {
    .only-mobile {
        display: none;
    }
}
@media (min-width: 300px) {
    .header,
    .rues,
    .fin {
        display: none !important;
    }
}

/* Other */

body,
html {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: monospace, sans-serif;
    background-color: #fff4c2;
    color: #413f00;
    scroll-behavior: smooth;
}

*::-webkit-scrollbar {
    display: none;
}

.racine {
    // border: 1px solid black;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    position: relative;

    .go-up {
        height: 30px;
        width: 30px;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 1.7em;
        transform: rotate(180deg);
        background-color: #ffffff86;
        box-shadow: 0px 0px 60px #28272755;
        border-radius: 0.2em;
        position: fixed;
        bottom: 0em;
        right: 0em;
        cursor: pointer;
        opacity: 0;
        transition: 500ms;
        text-decoration: none;
        color: #413f00;
        border: 1px solid #413f0040;
    }

    /* Header */
    .header {
        // border: 1px solid black;
        width: 100%;
        height: 200px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;

        /* input Seacrh */
        .search-box {
            font-family: monospace;
            border: 1px solid #413f0040;
            height: 3em;
            border-radius: 1em;
            width: 90%;
            outline: none;
            background-color: #ffffff86;
            box-shadow: 0px 0px 60px #3e3e3e1f;
            text-align: center;
        }
    }

    /* Rues */
    .rues {
        // border: 1px solid black;
        width: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 1em;
        padding: 0.5em 0;

        /* Rue */
        .rue {
            transition: 500ms;
            border: 1px solid #413f0040;
            width: 90%;
            height: 120px;
            // background-color: #ffffff86;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            border-radius: 1em;
            box-shadow: 0px 0px 60px #3e3e3e1f;

            /* Name */
            .rue-name {
                text-transform: uppercase;
                font-size: 5vw;
                color: #413f00;
            }

            /* Options */
            .optins-courrier {
                display: flex;
                justify-content: center;
                align-items: center;
                gap: 0.5em;

                label {
                    display: flex;
                    justify-content: center;
                    align-items: center;

                    span {
                        font-size: 0.6em;
                    }
                }
            }
        }
    }

    .fin {
        height: 150px;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;

        button {
            border: none;
            border-radius: 1em;
            height: 70px;
            width: 90%;
            cursor: pointer;
            border: 1px solid #413f0040;
            box-shadow: 0px 0px 60px #3e3e3e1f;
        }
    }
}
</style>
