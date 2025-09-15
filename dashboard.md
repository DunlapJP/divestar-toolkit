---
layout: default
title: Investment Dashboard
---

# Arkansas's Public Investments in Israel Bonds

This dashboard visualizes the data from our research, showing the timeline of total public funds committed by the state.

<div class="chart-container" style="height: 500px; width: 100%; margin-top: 40px; background-color: #fdfdfd; padding: 20px; border-radius: 12px; border: 1px solid #e0e0e0;">
  <canvas id="investmentChart"></canvas>
</div>

<script>
document.addEventListener("DOMContentLoaded", function() {
  // Use Jekyll to convert your CSV data into a JavaScript array directly.
  // This is faster and more reliable than fetching the file separately.
  const data = {{ site.data.investments | jsonify }};
  
  const investmentEvents = data
    .filter(row => {
      const type = row.Transaction_Type ? row.Transaction_Type.trim() : '';
      return row.Amount && (type === 'Peak Holding' || type === 'Authorization');
    })
    .sort((a, b) => new Date(a.Transaction_Date) - new Date(b.Transaction_Date));

  const chartDataPoints = [];
  const labelsForChart = [];
  
  // Find the earliest investment date to establish a starting point
  const firstInvestmentDate = investmentEvents.length > 0 ? new Date(investmentEvents[0].Transaction_Date) : new Date();
  const treasuryStartDate = new Date(firstInvestmentDate);
  treasuryStartDate.setDate(treasuryStartDate.getDate() - 1);

  // Add a zero starting point for the chart
  labelsForChart.push(treasuryStartDate.toISOString().split('T')[0]);
  chartDataPoints.push({
    y: 0,
    label: "Initial State",
    date: treasuryStartDate.toISOString().split('T')[0]
  });

  // Process the events from the data
  let runningTotal = 0;
  investmentEvents.forEach(event => {
    const amount = parseFloat(event.Amount);
    runningTotal += amount;
    labelsForChart.push(event.Transaction_Date);
    chartDataPoints.push({
      y: runningTotal,
      label: `${event.Entity.trim()} - ${event.Transaction_Type.trim()}`,
      date: event.Transaction_Date
    });
  });

  const ctx = document.getElementById('investmentChart').getContext('2d');
  new Chart(ctx, {
    type: 'line',
    data: {
      labels: labelsForChart,
      datasets: [{
        label: 'Total Public Funds Committed ($)',
        data: chartDataPoints.map(d => d.y),
        backgroundColor: 'rgba(217, 69, 69, 0.1)',
        borderColor: 'rgba(217, 69, 69, 1)',
        borderWidth: 2,
        pointBackgroundColor: 'rgba(217, 69, 69, 1)',
        pointRadius: 4,
        pointHoverRadius: 7,
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
              return '$' + new Intl.NumberFormat().format(value / 1000000) + 'M'; // Format as millions
            },
            font: {
                family: "'Source Sans Pro', sans-serif"
            }
          }
        },
        x: {
          ticks: {
            font: {
                family: "'Source Sans Pro', sans-serif"
            }
          }
        }
      },
      plugins: {
        legend: {
            labels: {
                font: {
                    family: "'Source Sans Pro', sans-serif",
                    size: 14
                }
            }
        },
        tooltip: {
          enabled: true,
          mode: 'index',
          intersect: false,
          callbacks: {
            title: function(tooltipItems) {
                const date = chartDataPoints[tooltipItems[0].dataIndex].date;
                return new Date(date).toLocaleDateString('en-US', { year: 'numeric', month: 'long', day: 'numeric' });
            },
            label: function(context) {
              let point = chartDataPoints[context.dataIndex];
              let amount = new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(context.parsed.y);
              return `${point.label}: ${amount}`;
            }
          }
        }
      }
    }
  });
});
</script>
```eof
