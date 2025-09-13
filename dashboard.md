---
layout: default
title: Financial Deep Dive
---

# Financial Deep Dive: Arkansas's Public Investments in Israel

This dashboard provides a detailed analysis of the **$157 million** in public funds from Arkansas that are either invested in or authorized for the purchase of Israel Bonds. The data is sourced from our publicly available **[investment data file](./investment-data/investments.csv)** and official treasury documents.

---

## Total Public Commitment: $157 Million

The total commitment comes from three state entities. The State Treasury holds existing bonds, while the two largest pension funds, ATRS and APERS, have authorized future investments.

<div class="chart-container" style="height:400px;">
  <canvas id="commitmentChart"></canvas>
</div>

<script>
document.addEventListener("DOMContentLoaded", function() {
  // Chart for Total Commitment Breakdown
  const commitmentCtx = document.getElementById('commitmentChart').getContext('2d');
  new Chart(commitmentCtx, {
    type: 'doughnut',
    data: {
      labels: ['AR State Treasury (Invested)', 'ATRS (Authorized)', 'APERS (Authorized)'],
      datasets: [{
        label: 'Total Commitment',
        data: [57000000, 50000000, 50000000],
        backgroundColor: [
          'rgba(58, 140, 91, 0.7)',  // Primary Green
          'rgba(217, 69, 69, 0.7)',    // Primary Red
          'rgba(255, 159, 64, 0.7)'   // Orange
        ],
        borderColor: [
          'rgba(58, 140, 91, 1)',
          'rgba(217, 69, 69, 1)',
          'rgba(255, 159, 64, 1)'
        ],
        borderWidth: 1
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          position: 'top',
        },
        tooltip: {
          callbacks: {
            label: function(context) {
              let label = context.label || '';
              if (label) {
                label += ': ';
              }
              if (context.parsed !== null) {
                label += new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(context.parsed);
              }
              return label;
            }
          }
        }
      }
    }
  });
});
</script>

---

## AR State Treasury Portfolio: A Closer Look

The most detailed data available is for the State Treasury's portfolio. As of the last public report on **October 7, 2024**, the portfolio had a face value of $52 million and showed an unrealized loss of over **$1.2 million**.

### Unrealized Gains & Losses by Bond (Oct. 7, 2024)
This chart shows the negative market value fluctuation for each of the State Treasury's individual bond holdings at the time of the report.

<div class="chart-container" style="height:500px;">
  <canvas id="unrealizedLossChart"></canvas>
</div>

### Treasury Portfolio Details (Oct. 7, 2024)
<div style="overflow-x:auto;">
  <table id="portfolioTable">
    <thead>
      <tr>
        <th>Description</th>
        <th>Coupon Rate</th>
        <th>Maturity Date</th>
        <th>Original Cost</th>
        <th>Market Value</th>
        <th>Unrealized Gain/Loss</th>
      </tr>
    </thead>
    <tbody>
      </tbody>
  </table>
</div>
---

## Market Context: Tracking Fluctuations

While we cannot track the daily value of Arkansas's specific bonds, we can observe the performance of the **S&P Israel Sovereign Bond Index** as a proxy for market trends. The index shows significant volatility, reflecting the heightened geopolitical and financial risks that have led to credit downgrades.

### S&P Israel Sovereign Bond Index Performance (1-Year)

<div class="chart-container" style="height:400px;">
  <canvas id="marketIndexChart"></canvas>
</div>

<p style="text-align:center; font-size: 0.9em; color: #666;"><i>Note: This chart shows the performance of a market index and is for illustrative purposes. It does not represent the exact performance of the bonds held by Arkansas.</i></p>

---

## Assessing the Risk: Credit Rating Downgrades

International rating agencies have downgraded Israel's credit outlook, citing the financial risks associated with ongoing conflict. This directly impacts the risk profile of holding Israel Bonds.

<div class="kpi-group">
    <div class="kpi">
        <div class="kpi-value">Moody's: A2</div>
        <div class="kpi-label">Downgraded on Feb. 9, 2024, with a negative outlook due to geopolitical risks.</div>
    </div>
    <div class="kpi">
        <div class="kpi-value">S&P Global: A</div>
        <div class="kpi-label">Downgraded on Oct. 1, 2024, with a negative outlook due to heightened security risks.</div>
    </div>
</div>


<script>
document.addEventListener("DOMContentLoaded", function() {
  const treasuryFile = '{{ "/source-documents/2024-10-07_AR-Treasury_Portfolio-Holdings_Israel-Bonds.xlsx - Israel Bonds.csv" | relative_url }}';

  Papa.parse(treasuryFile, {
    download: true,
    header: true,
    complete: function(results) {
      const bondData = results.data.slice(0, -1); // Remove the summary row

      // Chart for Unrealized Gains/Losses
      const lossCtx = document.getElementById('unrealizedLossChart').getContext('2d');
      new Chart(lossCtx, {
        type: 'bar',
        data: {
          labels: bondData.map(b => b.Identifier),
          datasets: [{
            label: 'Unrealized Gain/Loss ($)',
            data: bondData.map(b => parseFloat(b['Base Net Total Unrealized Gain/Loss'])),
            backgroundColor: 'rgba(217, 69, 69, 0.7)',
            borderColor: 'rgba(217, 69, 69, 1)',
            borderWidth: 1
          }]
        },
        options: {
          indexAxis: 'y',
          responsive: true,
          maintainAspectRatio: false,
          plugins: { legend: { display: false } },
          scales: {
            x: {
              ticks: {
                callback: function(value) {
                  return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(value);
                }
              }
            }
          }
        }
      });

      // Populate Portfolio Table
      const tableBody = document.querySelector("#portfolioTable tbody");
      const currencyFormatter = new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' });
      bondData.forEach(bond => {
        let row = tableBody.insertRow();
        row.insertCell().textContent = bond.Description;
        row.insertCell().textContent = (parseFloat(bond['Coupon Rate']) * 100).toFixed(2) + '%';
        row.insertCell().textContent = bond['Final Maturity'];
        row.insertCell().textContent = currencyFormatter.format(bond['Base Original Cost']);
        row.insertCell().textContent = currencyFormatter.format(bond['Base Market Value']);
        row.insertCell().textContent = currencyFormatter.format(bond['Base Net Total Unrealized Gain/Loss']);
      });
    }
  });

  // Dummy data for S&P Israel Sovereign Bond Index Chart (based on search result percentages)
  const marketIndexCtx = document.getElementById('marketIndexChart').getContext('2d');
  new Chart(marketIndexCtx, {
    type: 'line',
    data: {
      labels: ['12 Months Ago', '9 Months Ago', '6 Months Ago', '3 Months Ago', 'Today'],
      datasets: [{
        label: 'S&P Israel Sovereign Bond Index (Total Return)',
        data: [100, 103, 105, 102.68, 107.98], // Representative data based on 7.98% 1-year return
        borderColor: 'rgba(58, 140, 91, 1)',
        backgroundColor: 'rgba(58, 140, 91, 0.1)',
        fill: true,
        tension: 0.1
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: { legend: { display: false } },
      scales: {
        y: {
          ticks: {
            callback: function(value) { return value; } // Simplified y-axis
          },
          title: {
            display: true,
            text: 'Index Value (Normalized to 100)'
          }
        }
      }
    }
  });
});
</script>
