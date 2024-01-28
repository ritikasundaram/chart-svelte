<script>
	import { onMount } from "svelte";
	import Chart from "chart.js/auto";
  
	let ctx;
  
	onMount(async () => {
	  const res = await fetch("http://3.6.248.97:8000/v1_0/analytics/?param=service_options.type&op=sum&aggrparam=1&filter=&graph=simple&startdate=2023-08-07T00:00:00&enddate=2023-08-12T00:00:00", {method:'POST',
	  headers: {
			  'Content-Type': 'application/json',
		  },
	  body: JSON.stringify(
		{
    "filter": {
        "tracking_status": "Return",
        "account_info.user_id": [
            "6278d4ca0afce063c79fb363"
        ]
    }
}
			  )
		  })
	  const json = await res.json()
  
	  const service_options_type = json.detail.data.map(item => item.service_options_type);
	  const counts = json.detail.data.map(item => item.count);
  
	  new Chart(ctx, {
		type: "pie",
		data: {
		  labels: service_options_type,
		  datasets: [
			{
			  label: "Count",
			  data: counts,
			  borderWidth: 1,
			},
		  ],
		},
		options: {
		  scales: {
			y: {
			  beginAtZero: true,
			},
		  },
		},
	  });
	});
  </script>
  
  <div>
	<canvas id="myChart" bind:this={ctx}></canvas>
  </div>
  