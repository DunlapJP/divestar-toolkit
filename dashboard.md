---
layout: default
title: Investment Dashboard
---

# Arkansas's Public Investments in Israel Bonds

This dashboard visualizes the data from our research, showing the total public funds committed by various state entities. The chart below is generated directly from our publicly available **[investment data file](./investment-data/investments.csv)**.

<div class="chart-container">
  <canvas id="investmentChart"></canvas>
</div>

<script>
document.addEventListener("DOMContentLoaded", function() {
  // Path to your CSV data file
  const csvFile = '{{ "/investment-data/investments.csv" | relative_url }}';

  // Use PapaParse to fetch and parse the CSV file
  Papa.parse(csvFile, {
    download: true,
    header: true,
    complete: function(results) {
      const data = results.data;
      
      // Process the data to sum amounts by each entity
      const investmentData = data.reduce((acc, row) => {
        const entity = row.Entity;
        const amount = parseFloat(row.Amount);

        // We only want to sum entries that are currently held or authorized
        if (entity && !isNaN(amount) && (row.Status === 'Held' || row.Status === 'Authorized')) {
          if (!acc[entity]) {
            acc[entity] = 0;
          }
          acc[entity] += amount;
        }
        return acc;
      }, {});

      const labels = Object.keys(investmentData);
      const values = Object.values(investmentData);

      // Get the canvas element to draw the chart on
      const ctx = document.getElementById('investmentChart').getContext('2d');
      
      // Create the bar chart using Chart.js
      new Chart(ctx, {
        type: 'bar',
        data: {
          labels: labels,
          datasets: [{
            label: 'Total Investment/Authorization ($)',
            data: values,
            backgroundColor: 'rgba(217, 69, 69, 0.7)', // Using your --primary-red with transparency
            borderColor: 'rgba(217, 69, 69, 1)',
            borderWidth: 1
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            y: {
              beginAtZero: true,
              ticks: {
                // Format the y-axis labels as currency
                callback: function(value) {
                  return '$' + new Intl.NumberFormat().format(value);
                }
              }
            }
          },
          plugins: {
            legend: {
              display: false // Hide the legend as it's self-explanatory
            },
            tooltip: {
              // Format the tooltip that appears on hover
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
