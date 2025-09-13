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
  const csvFile = '{{ "/investment-data/investments.csv" | relative_url }}';

  Papa.parse(csvFile, {
    download: true,
    header: true,
    complete: function(results) {
      const data = results.data;
      
      // This logic now specifically looks for the most definitive commitment amounts
      const investmentData = data.reduce((acc, row) => {
        const entity = row.Entity;
        const amount = parseFloat(row.Amount);
        const type = row.Transaction_Type;

        // We only want to chart the peak holdings and authorizations
        if (entity && !isNaN(amount) && (type === 'Peak Holding' || type === 'Authorization')) {
            acc[entity] = amount;
        }
        return acc;
      }, {});

      const labels = Object.keys(investmentData);
      const values = Object.values(investmentData);

      const ctx = document.getElementById('investmentChart').getContext('2d');
      new Chart(ctx, {
        type: 'bar',
        data: {
          labels: labels,
          datasets: [{
            label: 'Total Investment/Authorization ($)',
            data: values,
            backgroundColor: 'rgba(217, 69, 69, 0.7)',
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
                callback: function(value) {
                  return '$' + new Intl.NumberFormat().format(value);
                }
              }
            }
          },
          plugins: {
            legend: {
              display: false
            },
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
