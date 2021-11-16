<template>
  <div id="body">
    <section class="section">
      <h1 class="title is-1">Special Schedule Generator</h1>
      <nav class="level">
        <div class="level-left">
          <div class="level-item">
            <b-field label="Special schedule date" label-position="on-border">
                <b-datepicker v-model="special_schedule_date"></b-datepicker>
            </b-field>
          </div>
          <div class="level-item">
            <b-button type="is-primary" @click="copy_special_schedule()">Copy Entry</b-button>
          </div>
        </div>
      </nav>
      <nav class="level">
        <div class="level-left">
          <div class="level-item">
            <b-field label="Block name" label-position="on-border">
              <b-input v-model="special_schedule_block"></b-input>
            </b-field>
          </div>
          <div class="level-item">
            <b-field label="Start time" label-position="on-border">
              <b-timepicker v-model="special_schedule_start"></b-timepicker>
            </b-field>
          </div>
          <div class="level-item">
            <b-field label="End time" label-position="on-border">
              <b-timepicker v-model="special_schedule_end"></b-timepicker>
            </b-field>
          </div>
          <div class="level-item">
            <b-button type="is-primary" @click="add_special_schedule()">Add</b-button>
          </div>
        </div>
      </nav>
      <nav class="level" v-for="(start_end, name) in special_schedule_dict" :key="name">
        <div class="level-left">
          <div class="level-item">
            <p>
              Block: {{ name }} | Start: {{ start_end[0][0] + ":" + start_end[0][1]}} | End: {{ start_end[1][0] + ":" + start_end[1][1]}}
            </p>
          </div>
        </div>
        <div class="level-right">
          <div class="level-item">
            <b-button type="is-primary" @click="remove_special_schedule_entry(name)">Remove</b-button>
          </div>
        </div>
      </nav>
      <div class="content" type="is-white">
        <pre class="grey">
{{ '"' + special_schedule_date.getFullYear().toString() + "/" + (special_schedule_date.getMonth() + 1).toString() + "/" + special_schedule_date.getDate().toString() + '": ' + JSON.stringify(special_schedule_dict) }}</pre>
      </div>
    </section>
    <section class="section">
      <h1 class="title is-1">Normal Schedule Display</h1>
      <div class="tile is-ancestor">
        <div class="tile is-parent" v-for="day in Object.keys(schedule)" :key="day">
          <article class="tile is-child notification is-danger">
            <p class="title">{{ day }}</p>
          </article>
        </div>
      </div>
      <div class="tile is-ancestor">
        <div class="tile is-parent is-vertical" v-for="(schedule_, day) in schedule" :key="day">
          <article class="tile is-child notification is-danger" v-for="(start_end, block) in schedule_" :key="block">
            <p class="title">{{ block }}</p>
            <p class="subtitle">{{ start_end[0][0] + ":" + start_end[0][1] + "-" + start_end[1][0] + ":" + start_end[1][1]}}</p>
          </article>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      special_schedule_date: new Date(),
      special_schedule_block: "",
      special_schedule_start: new Date(),
      special_schedule_end: new Date(),
      special_schedule_dict: {},
      schedule: {"Monday":{"Morning Meeting":[[8,30],[8,50]],"A":[[8,55],[10,10]],"B":[[10,15],[11,30]],"Lunch":[[11,30],[12,30]],"C":[[12,30],[13,45]],"D":[[13,50],[15,5]],"Tutorial":[[15,5],[15,35]]},"Tuesday":{"Group Advisory/1-on-1s":[[8,30],[8,50]],"E":[[8,55],[10,10]],"F":[[10,15],[11,30]],"Lunch":[[11,30],[12,30]],"A":[[12,30],[13,45]],"B":[[13,50],[15,5]],"Tutorial":[[15,5],[15,35]]},"Wednesday":{"Morning Meeting":[[8,30],[8,50]],"C":[[8,55],[10,10]],"D":[[10,15],[11,30]],"Lunch":[[11,30],[12,30]],"E":[[12,30],[13,45]],"F":[[13,50],[15,5]],"Tutorial":[[15,5],[15,35]]},"Thursday":{"Group Advisory/1-on-1s":[[8,30],[8,50]],"B":[[8,55],[10,25]],"A":[[10,35],[12,5]],"Lunch":[[12,5],[12,55]],"C":[[12,55],[14,25]],"Tutorial":[[14,25],[14,55]]},"Friday":{"Morning Meeting":[[8,30],[8,50]],"D":[[8,55],[10,25]],"F":[[10,35],[12,5]],"Lunch":[[12,5],[12,55]],"E":[[12,55],[14,25]]}}
    }
  },
  methods: {
    add_special_schedule() {
      this.special_schedule_dict[this.special_schedule_block] = [[this.special_schedule_start.getHours(), this.special_schedule_start.getMinutes()], [this.special_schedule_end.getHours(), this.special_schedule_end.getMinutes()]]
      this.special_schedule_block = ""
    },
    copy_special_schedule() {
      navigator.clipboard.writeText('"' + this.special_schedule_date.getFullYear().toString() + "/" + (this.special_schedule_date.getMonth() + 1).toString() + "/" + this.special_schedule_date.getDate().toString() + '": ' + JSON.stringify(this.special_schedule_dict));
    },
    remove_special_schedule_entry(entry) {
      const temp = { ...this.special_schedule_dict };
      delete temp[entry];
      this.special_schedule_dict = temp
    }
  }
}
</script>
