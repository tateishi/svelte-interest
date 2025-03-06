<script lang="ts">
    const national_tax_rate: number = 15.315;
    const local_tax_rate: number = 5;
    const days_per_year: number = 365;

    let amount: number = 1000000;
    let rate: number = 0.2;
    let date_from: string = getToday();
    let date_to: string = yearsAfter(date_from);

    $: interest = amount * rate * days / days_per_year / 100;
    $: national_tax = Math.floor(interest * national_tax_rate / 100);
    $: local_tax = Math.floor(interest * local_tax_rate / 100);
    $: total_tax = national_tax + local_tax;
    $: net_interest = interest - total_tax;
    $: total = Number(amount) + Number(net_interest);
    $: days = Math.floor((Number(new Date(date_to)) - Number(new Date(date_from))) / (1000 * 60 * 60 * 24));

    $: console.log(`${date_from} and ${date_to}`);
    $: console.log(`${days} days`);

    function getToday() : string {
        let today: string = new Date().toLocaleDateString();
        let a = today.split("/");
        today = `${a[0]}-${a[1].padStart(2, "0")}-${a[2].padStart(2, "0")}`;
        return today;
    }

    function yearsAfter(date:string, n: number = 1):string {
        let a = date.split("-");
        let year = Number(a[0]) + n;
        let month = a[1];
        let day = a[2];
        return `${year}-${month.padStart(2, "0")}-${day.padStart(2, "0")}`;
    }

    function monthsAfter(date: string, n: number = 1): string {
        let a = date.split("-");
        let year = Number(a[0]);
        let month = Number(a[1]) + n;
        if (month > 12) {
            year += 1;
            month -= 12;
        }
        let day = a[2];
        return `${year}-${month.toString().padStart(2, "0")}-${day.padStart(2, "0")}`;
    }

    function setYearsAfter(n: number) {
        date_to = yearsAfter(date_from, n);
    }

    function setMonthsAfter(n: number) {
        date_to = monthsAfter(date_from, n);
    }
</script>

<div class="text-center text-orange-500">
    <label for="amount">元金:</label>
    <input id="amount" bind:value={amount} />
</div>
<div class="text-center text-orange-500">
    <label for="rate">金利(年):</label>
    <input id="rate" bind:value={rate} />
</div>
<div class="text-center text-orange-500">
    <label for="date_from">開始日:</label>
    <input id="date_from" type="date" bind:value={date_from} /> 
</div>
<div class="text-center text-orange-500">
    <label for="date_to">終了日:</label>
    <input id="date_to" type="date" bind:value={date_to} />
</div>
<div class="text-center text-orange-500">
    <button on:click={()=>setYearsAfter(1)}>1年後</button>
    <button on:click={()=>setYearsAfter(2)}>2年後</button>
    <button on:click={()=>setYearsAfter(3)}>3年後</button>
    <button on:click={()=>setYearsAfter(5)}>5年後</button>
</div>
<div class="text-center text-orange-500">
    <button on:click={()=>setMonthsAfter(1)}>1ヶ月後</button>
    <button on:click={()=>setMonthsAfter(2)}>2ヶ月後</button>
    <button on:click={()=>setMonthsAfter(3)}>3ヶ月後</button>
    <button on:click={()=>setMonthsAfter(6)}>6ヶ月後</button>
</div>

<div class="text-center text-orange-500">
    <label for="gankin">元金:</label>
    <output id="gankin">{amount.toLocaleString()}</output>      
</div>
<div class="text-center text-orange-500">
    <label for="days">日数:</label>
    <output id="days">{days.toLocaleString()}</output>
</div>
<div class="text-center text-orange-500">
    <label for="interest">利息:</label>
    <output id="interest">{Math.floor(interest).toLocaleString()}</output>
</div>
<div class="text-center text-orange-500">
    <label for="national_tax">国税:</label>
    <output id="national_tax">{national_tax.toLocaleString()}</output>
</div>
<div class="text-center text-orange-500">
    <label for="local_tax">地方税:</label>
    <output id="local_tax">{local_tax.toLocaleString()}</output>
</div>
<div class="text-center text-orange-500">
    <label for="total_tax">税金:</label>
    <output id="total_tax">{total_tax.toLocaleString()}</output> 
</div>
<div class="text-center text-orange-500">
    <label for="net_interest">税引後利息:</label>
    <output id="net_interest">{Math.floor(net_interest).toLocaleString()}</output>
</div>
<div class="text-center text-orange-500">
    <label for="total">合計:</label>
    <output id="total">{Math.floor(total).toLocaleString()}</output>
</div>
