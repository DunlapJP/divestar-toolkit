---
layout: default
title: Investment Dashboard
---

# Arkansas's Public Investments in Israel Bonds

This dashboard visualizes the data from our research, showing the timeline of total public funds committed by the state. The chart below is generated directly from our publicly available **[investment data file](./investment-data/investments.csv)**.

<div class="chart-container">
  <canvas id="investmentChart"></canvas>
</div>

<script>
document.addEventListener("DOMContentLoaded", function() {
  const csvFile = '{{ "/investment-data/investments.csv" | relative_url }}';

  Papa.parse(csvFile, {
    download: true,
    header: true,
    complete: function(results) {
      const data = results.data;
      
      // Filter for relevant investment events and sort by date
      const investmentEvents = data
        .filter(row => {
          const type = row.Transaction_Type ? row.Transaction_Type.trim() : '';
          return row.Amount && (type === 'Peak Holding' || type === 'Authorization');
        })
        .sort((a, b) => new Date(a.Transaction_Date) - new Date(b.Transaction_Date));

      let cumulativeAmount = 0;
      const chartData = investmentEvents.map(event => {
        const amount = parseFloat(event.Amount);
        cumulativeAmount += amount;
        return {
          date: event.Transaction_Date,
          amount: cumulativeAmount,
          label: `${event.Entity.trim()} - ${event.Transaction_Type.trim()}`
        };
      });

      // Find the earliest investment date to establish a starting point
      const firstInvestmentDate = investmentEvents.length > 0 ? new Date(investmentEvents[0].Transaction_Date) : new Date();
      const treasuryStartDate = new Date(firstInvestmentDate);
      treasuryStartDate.setDate(treasuryStartDate.getDate() - 1); // Set start point just before the first investment

      const initialTreasuryValue = 57000000; // Peak holding amount from the first relevant event

      // Manually add the initial State Treasury holding as the base
      const finalChartData = [
          { date: treasuryStartDate.toISOString().split('T')[0], amount: 0, label: "Initial State" },
          { date: investmentEvents[0].Transaction_Date, amount: initialTreasuryValue, label: investmentEvents[0].Entity.trim() + " Peak Holding" }
      ];
      
      let runningTotal = initialTreasuryValue;
      // Add subsequent authorizations
      for(let i = 1; i < investmentEvents.length; i++) {
          runningTotal += parseFloat(investmentEvents[i].Amount);
          finalChartData.push({
              date: investmentEvents[i].Transaction_Date,
              amount: runningTotal,
              label: investmentEvents[i].Entity.trim() + " Authorization"
          });
      }


      const labels = finalChartData.map(d => d.date);
      const values = finalChartData.map(d => d.amount);

      const ctx = document.getElementById('investmentChart').getContext('2d');
      new Chart(ctx, {
        type: 'line',
        data: {
          labels: labels,
          datasets: [{
            label: 'Total Public Funds Committed ($)',
            data: values,
            backgroundColor: 'rgba(217, 69, 69, 0.2)',
            borderColor: 'rgba(217, 69, 69, 1)',
            borderWidth: 3,
            fill: true,
            tension: 0.1
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            y: {
              beginAtZero: true,
              ticks: {
                callback: function(value) {
                  return '$' + new Intl.NumberFormat().format(value);
                }
              }
            }
          },
          plugins: {
            tooltip: {
              callbacks: {
                label: function(context) {
                  let label = context.dataset.label || '';
                  if (label) {
                    label += ': ';
                  }
                  if (context.parsed.y !== null) {
                    label += new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(context.parsed.y);
                  }
                  return label;
                }
              }
            }
          }
        }
      });
    }
  });
});
</script>
