---
layout: tailwind 
title: Statistics 
search_exclude: true
permalink: /stats/
---
  <title>Wildfire Statistics Dashboard</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 2rem;
      background-color: #f9f9f9;
    }
    h1 {
      margin-bottom: 1rem;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 1rem;
      background: #fff;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }
    th, td {
      padding: 0.75rem;
      border: 1px solid #ccc;
      text-align: right;
    }
    th {
      background-color: #f0f0f0;
    }
    .stats-summary {
      margin-top: 2rem;
      background: #e9f5e9;
      padding: 1rem;
      border-left: 5px solid #4caf50;
    }
  </style>
  <h1>Total Wildland Fires and Acres (1983–2024)</h1>
  <div class="stats-summary" id="summary"></div>
  <button id="toggle-table-btn" style="margin-top:1rem;">Show Data Table</button>
  <div id="table-container" style="display:none;">
    <table>
      <thead>
        <tr>
          <th>Year</th>
          <th>Fires</th>
          <th>Acres Burned</th>
        </tr>
      </thead>
      <tbody id="data-table"></tbody>
    </table>
  </div>
  <img src="images/Screenshot 2025-05-21 at 10.00.16 PM.png" alt="Human Caused Fires" style="height:100px;">

  <script>
    const rawData = `
2024	64897	8924884
2023	56580	2693910
2022	68988	7577183
2021	58985	7125643
2020	58950	10122336
2019	50477	4664364
2018	58083	8767492
2017	71499	10026086
2016	67743	5509995
2015	68151	10125149
2014	63312	3595613
2013	47579	4319546
2012	67774	9326238
2011	74126	8711367
2010	71971	3422724
2009	78792	5921786
2008	78979	5292468
2007	85705	9328045
2006	96385	9873745
2005	66753	8689389
2004	65461	8097880
2003	63629	3960842
2002	73457	7184712
2001	84079	3570911
2000	92250	7393493
1999	92487	5626093
1998	81043	1329704
1997	66196	2856959
1996	96363	6065998
1995	82234	1840546
1994	79107	4073579
1993	58810	1797574
1992	87394	2069929
1991	75754	2953578
1990	66481	4621621
1989	48949	1827310
1988	72750	5009290
1987	71300	2447296
1986	85907	2719162
1985	82591	2896147
1984	20493	1148409
1983	18229	1323666
    `;

    const data = rawData.trim().split('\n').map(row => {
      const [year, fires, acres] = row.split('\t');
      return {
        year: parseInt(year),
        fires: parseInt(fires.replace(/\*/g, '')),
        acres: parseInt(acres.replace(/\*/g, ''))
      };
    });

    // Only render the table when requested
    function renderTable() {
      const tableBody = document.getElementById("data-table");
      tableBody.innerHTML = "";
      data.forEach(row => {
        const tr = document.createElement("tr");
        tr.innerHTML = `<td>${row.year}</td><td>${row.fires.toLocaleString()}</td><td>${row.acres.toLocaleString()}</td>`;
        tableBody.appendChild(tr);
      });
    }

    // Calculate stats
    const totalYears = data.length;
    const totalFires = data.reduce((sum, row) => sum + row.fires, 0);
    const totalAcres = data.reduce((sum, row) => sum + row.acres, 0);
    const avgFires = Math.round(totalFires / totalYears);
    const avgAcres = Math.round(totalAcres / totalYears);

    const maxFireYear = data.reduce((max, row) => row.fires > max.fires ? row : max);
    const maxAcresYear = data.reduce((max, row) => row.acres > max.acres ? row : max);

    document.getElementById("summary").innerHTML = `
      <strong>Averages (1983–2024):</strong><br>
      🔥 Average Fires per Year: <strong>${avgFires.toLocaleString()}</strong><br>
      🌾 Average Acres Burned per Year: <strong>${avgAcres.toLocaleString()}</strong><br><br>
      📈 Highest Number of Fires: <strong>${maxFireYear.fires.toLocaleString()}</strong> in ${maxFireYear.year}<br>
      🔥 Most Acres Burned: <strong>${maxAcresYear.acres.toLocaleString()}</strong> in ${maxAcresYear.year}
    `;

    // Toggle table visibility
    const toggleBtn = document.getElementById("toggle-table-btn");
    const tableContainer = document.getElementById("table-container");
    let tableVisible = false;
    toggleBtn.addEventListener("click", () => {
      tableVisible = !tableVisible;
      if (tableVisible) {
        renderTable();
        tableContainer.style.display = "block";
        toggleBtn.textContent = "Hide Data Table";
      } else {
        tableContainer.style.display = "none";
        toggleBtn.textContent = "Show Data Table";
      }
    });
  </script>
