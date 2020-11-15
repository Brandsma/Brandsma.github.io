<template>
  <div class="risk-interface" :class="{ loading: loading }">
    <header class="center card">
      <h1>Welcome to Rafa Demo Interface!</h1>
      <button v-on:click="getAssessment">Click me!</button>
    </header>
    <div class="risk-container">
      <div class="loading-screen">
        <p>LOADING...</p>
      </div>
      <div class="map-container" v-if="ticket !== null">
        <div class="card">
          <header>
            <h2 class="center">Emergency</h2>
          </header>
          <p class="center">
            {{ ticket["melding_1"] }} -- {{ ticket["melding_2"] }} --
            {{ ticket["melding_3"] }}
          </p>
          <b>Location:</b>
          <p>
            <i>{{ ticket["location"] }}</i>
          </p>
        </div>
        <iframe
          width="425"
          height="350"
          frameborder="0"
          scrolling="no"
          marginheight="0"
          marginwidth="0"
          :src="genMapsLink()"
          style="border: 1px solid black"
        />
      </div>
      <div v-if="ticket !== null">
        <div class="card">
          <header>
            <h2 class="center">Assessment</h2>
          </header>
          <section class="risk-bar" :class="'risk-' + this.riskLevel">
            <h3>
              Overall Risk <span class="label">Level {{ this.riskLevel }}</span>
            </h3>
            <p>{{ riskExplanation }}</p>
          </section>

          <section
            class="risk"
            v-for="(risk, key) of this.risks"
            :key="key"
            :class="'risk-' + risk['risk_level']"
          >
            <header>
              <h3>{{ risk["pretty_name"] }}</h3>
              <div class="var-bar">
                <span class="label"> Level {{ risk["risk_level"] }} </span>
                <span class="label"> Weight {{ riskWeights[key] }} </span>
                <span class="label"> Indicator </span>
              </div>
            </header>
            <p>{{ risk["explanation"] }}</p>
          </section>

          <section class="risk-bar" :class="'risk-' + this.urgencyLevel">
            <h3>
              Overall Urgency
              <span class="label">Level {{ this.urgencyLevel }}</span>
            </h3>
            <p>{{ urgencyExplanation }}</p>
          </section>

          <section
            class="risk"
            v-for="(urgency, key) of this.urgencies"
            :key="key"
            :class="'risk-' + urgency['urgency_level']"
          >
            <header>
              <h3>{{ urgency["pretty_name"] }}</h3>
              <div class="var-bar">
                <span class="label"> Level {{ urgency["urgency_level"] }}</span>
                <span class="label"> Weight {{ urgencyWeights[key] }} </span>
                <span class="label"> Indicator </span>
              </div>
            </header>
            <p>{{ urgency["explanation"] }}</p>
          </section>
        </div>
      </div>
      <div v-if="!loading && ticket == null" class="disclaimer-container">
        <div class="disclaimer">
          <section class="disclaimer-body">
            <h2 class="center">Disclaimer</h2>
            <p>
              Welcome to the Rafa Demo! This visual representation demonstrates
              a few of the capabilities of our risk assessment system. This
              visual front end is <b>NOT</b> the actual product, but only
              presents its features. By clicking the above button you will
              retrieve a mocked-up emergency GMS system ticket, realtime
              geospatial data and additional relevant data. This data is used in
              our system in order to calculate risk levels of the emergency and
              certain so-called risk indicators. <br /><br />
              To the left we show some of the general information about this
              emergency, such as location and type of emergency. To the right,
              the result of our risk assessment is shown. It displays an overall
              risk level along with suggested actions and below it a break down
              of the various risk indicators involved.
              <br /><br />
              The product that Rafa boasts is the automated risk assessment
              based on (geo-spatial) data extracted from the emergency ticket
              and enriched by various data sources. The gathered information is
              then classified using risk indicator models that should be crafted
              in conjunction with domain experts. The automated risk assessment
              created by Rafa can then be used by, among other things, civilian
              participation apps to determine whether and which civilians should
              be activated. We combine data from various sources into one much
              higher quality assessment.
            </p>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "RiskInterface",
  data() {
    return {
      loading: false,

      riskLevel: -1,
      riskExplanation: "",
      risks: null,
      ticket: null,
      riskIndicators: null,
      situationalWeights: null
    };
  },
  methods: {
    getAssessment() {
      this.riskLevel = -1;
      this.riskExplanation = "";
      this.risks = null;
      this.ticket = null;
      this.riskIndicators = null;
      this.riskWeights = null;
      this.urgencyLevel = -1;
      this.urgencyIndicators = null;
      this.urgencyExplanation = "";
      this.urgencyWeights = null;
      this.urgencies = null;

      this.loading = true;

      fetch("http://3.80.19.1/risk")
        .then(response => response.json())
        .then(data => {
          console.log(data);
          this.ticket = data["ticket"];
          this.riskLevel = data["risk_level"];
          this.riskExplanation = data["risk_explanation"];
          this.riskIndicators = data["risk_indicators"];
          this.risks = data["relative_risks"];
          this.riskWeights = data["current_risk_weights"];
          this.urgencyLevel = data["urgency_level"];
          this.urgencyExplanation = data["urgency_explanation"];
          this.urgencyIndicators = data["urgency_indicators"];
          this.urgencyWeights = data["current_urgency_weights"];
          this.urgencies = data["relative_urgencies"];
          this.loading = false;
        });
    },
    genMapsLink() {
      if (this.ticket !== null) {
        let lon = this.ticket["coords"][1];
        let lat = this.ticket["coords"][0];
        let leftTop = [lon - 0.002, lat - 0.002];
        let rightBottom = [lon + 0.002, lat + 0.002];
        return (
          "https://www.openstreetmap.org/export/embed.html?bbox=" +
          leftTop[0] +
          "%2C" +
          leftTop[1] +
          "%2C" +
          rightBottom[0] +
          "%2C" +
          rightBottom[1] +
          "&amp;layer=mapnik&amp;"
        );
      }
      return "https://www.openstreetmap.org/export/embed.html?bbox=6.552810966968537%2C53.21539088411063%2C6.5563514828681955%2C53.2167753453776&amp;layer=mapnik";
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.loading-screen {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.4);
  opacity: 0;
  transition: 1s ease;
  font-size: 1.5rem;
  font-weight: 800;
  border-radius: 1.5rem;
}

.loading .loading-screen {
  opacity: 1;
  border-radius: 1.5rem;
}

.disclaimer-container {
  display: flex;
  align-items: center;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.disclaimer {
  background-color: rgba(0, 0, 0, 0.4);
  border-radius: 1.5rem;
  overflow: auto;
  width: 100%;
  max-width: 700px;
  box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.1);
}

.disclaimer-body {
  text-align: justify;
  border-left: 1rem solid #f3a047;
  padding: 2rem;
}
</style>
