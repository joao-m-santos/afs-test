<template>
  <div class="home">
    <h1>This is a table with some important data</h1>
    <b-table
      :data="tableData"
      :columns="columns"
      :row-class="_getRowClass"
    ></b-table>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { TableData } from "@/types/types";

@Component
export default class Home extends Vue {
  tableData: TableData[] = [];
  columns = [
    {
      label: "Security class",
      field: "name"
    },
    {
      label: "Authorized amount",
      field: "authorizedAmount"
    },
    {
      label: "Issued amount",
      field: "issuedAmount"
    },
    {
      label: "Authorized Capital",
      field: "authorizedCapital"
    },
    {
      label: "Issued capital",
      field: "issuedCapital"
    }
  ];
  loading = false;

  // mounted works fine if your ide complains about it
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  async mounted() {
    this.loading = true;
    try {
      // Fetch data
      let data = await this.getData();

      // Add `randomNumber` field
      data = data.map((dataItem: TableData) => {
        return {
          ...dataItem,
          randomNumber: Math.random()
        };
      });

      // Calculate totals
      data.push(this._getTotalRow(data));

      // Assign to `tableData`
      this.tableData = data;

      this.loading = false;
    } catch (error) {
      console.log(error, "This is not good");
    }
  }

  async getData(): Promise<TableData[]> {
    return [
      {
        id: "42f2462d-49d0-4e91-8fe1-de2e656b0f06",
        name: "Series A",
        nominalValue: 5,
        authorizedAmount: 1500,
        issuedAmount: 500,
        authorizedCapital: 7550,
        issuedCapital: 2500
      },
      {
        id: "42f2462d-49d0-4e91-8fe1-de2e656b0f06",
        name: "Series B",
        nominalValue: 10,
        authorizedAmount: 15000,
        issuedAmount: 5000,
        authorizedCapital: 150000,
        issuedCapital: 50000
      },
      {
        id: "fd78c11b-e3d2-455a-99b0-49907a75c463",
        name: "Series C",
        nominalValue: 1,
        authorizedAmount: 96876,
        issuedAmount: 61760,
        authorizedCapital: 96876,
        issuedCapital: 61760
      },
      {
        id: "d8654cb0-8986-4fbc-b969-025e514cb934",
        name: "Series D",
        nominalValue: 1,
        authorizedAmount: 10110,
        issuedAmount: 1100,
        authorizedCapital: 10110,
        issuedCapital: 1100
      }
    ];
  }

  _getTotalRow(data: TableData[]): TableData {
    return data?.reduce(
      (acc, curr) => {
        acc.authorizedAmount += curr.authorizedAmount;
        acc.issuedAmount += curr.issuedAmount;
        acc.authorizedCapital += curr.authorizedCapital;
        acc.issuedCapital += curr.issuedCapital;

        return acc;
      },
      {
        id: "totalRow",
        name: "Total",
        nominalValue: 0,
        authorizedAmount: 0,
        issuedAmount: 0,
        authorizedCapital: 0,
        issuedCapital: 0
      }
    );
  }

  _getRowClass(row: TableData): string {
    return row.id === "totalRow" ? "has-text-weight-bold" : "";
  }
}
</script>
