<template>
  <div>
    <div class="grid">
      <vue-good-table
        :columns="gridColumns"
        :rows="hData"
        theme="black-rhino"
        styleClass="vgt-table striped"
      >
        <div slot="emptystate">loading...</div>
      </vue-good-table>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

interface IGeometry {
  type: string;
  coordinates: number[];
}

interface IProperty {
  label: string;
  score: number;
  housenumber: string;
  id: string;
  name: string;
  postcode: string;
  citycode: string;
  x: number;
  y: number;
  city: string;
  context: string;
  type: string;
  importance: number;
  street: string;
}

interface IFeature {
  type: string;
  geometry: IGeometry;
  properties: IProperty;
}

interface IHData {
  type: string;
  version: string;
  features: IFeature[];
}

interface IColumn {
  label: string,
  field: string
}

@Component({})
export default class Grid extends Vue {
  public gridColumns: IColumn[] = <IColumn[]>[
    { label: "id", field: "id" },
    { label: "name", field: "name" },
    { label: "street", field: "street" },
    { label: "house no.", field: "housenumber" },
  ];
  public hData: IProperty[] = <IProperty[]>[];
  public async fetchData(): Promise<IHData> {
    let aData = await this.$axios.get(
      "https://api-adresse.data.gouv.fr/search/?q=8+bd+du+port&limit=200"
    );
    let outData: IHData = aData.data;
    // [DEBUG-ONLY] if (outData) console.log(outData);
    return <IHData>outData;
  }

  mounted() {
    this.fetchData().then((data: IHData) => {
      this.hData = data.features.map((feature) => {
        return feature.properties;
      });
    });
  }
}
</script>