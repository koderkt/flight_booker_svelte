<script lang="ts">
    type Options = "one-way" | "return";

    function getDate() {
        let date = new Date();
        const [month, day, year] = date
            .toLocaleDateString("en-US", {
                year: "numeric",
                month: "2-digit",
                day: "2-digit",
            })
            .split("/");
        return `${year}-${month}-${day}`;
    }

    function handleSubmit(e: Event) {
        e.preventDefault();
        alert("Flight booked successfully!");
    }

    let selected = $state<Options>("one-way");
    let startDate = $state(getDate());
    let returnDate = $state(getDate());
</script>

<form
    onsubmit={handleSubmit}
    class="flex flex-col items-center justify-center min-h-screen bg-gray-100 p-4"
>
    <div class="w-full max-w-md bg-white rounded-lg shadow-lg p-8 space-y-6">
        <select
            bind:value={selected}
            class="w-full border border-gray-300 rounded-lg p-2 bg-white focus:outline-none focus:ring-2 focus:ring-blue-500"
        >
            <option value="one-way">one-way flight</option>
            <option value="return">return flight</option>
        </select>

        <label class="block">
            <span class="text-gray-700 font-medium">Select the Start Date</span>
            <input
                type="date"
                bind:value={startDate}
                required
                min={getDate()}
                class="w-full mt-2 border border-gray-300 rounded-lg p-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
        </label>

        <label class="block">
            <span class="text-gray-700 font-medium">Select the End Date</span>
            <input
                type="date"
                bind:value={returnDate}
                required
                disabled={selected != "return"}
                min={getDate()}
                class="w-full mt-2 border border-gray-300 rounded-lg p-2 focus:outline-none focus:ring-2 focus:ring-blue-500 disabled:bg-gray-100"
            />
        </label>

        <button
            type="submit"
            disabled={!startDate ||
                (selected === "return" && returnDate < startDate)}
            class="w-full bg-blue-600 text-white font-bold py-2 rounded-lg hover:bg-blue-700 disabled:opacity-50 disabled:cursor-not-allowed"
        >
            Submit
        </button>
    </div>
</form>
