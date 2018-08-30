<template>
  <div class="hello">
    <div class="row align-items-center">
      <div class="container-fluid h-100">
        <div class="row justify-content-center mt-4 mb-4">
          <div class="col-12 col-lg-8">
              <div class="bg-translucent text-white text-raleway pb-4">
                <div class="row justify-content-center">
                  <h1 class="header text-center pt-4 text-uppercase">Adventurers League<br>Season 8 Converter</h1>
                </div>
                <div class="row justify-content-center mt-4">
                  <div class="col-10 col-md-5 text-center my-auto">
                    <label class="" for="charXp">Character Experience Points</label>
                  </div>
                  <div class="col-10 col-md-5">
                    <input v-model="charXP" type="number" class="h-100 form-control" id="charXp" placeholder="17000">
                  </div>
                </div>
                <div class="row justify-content-center mt-4">
                  <div class="col-10">
                    <div class="row justify-content-center">
                      <button v-on:click="getAP" type="button" data-toggle="modal" data-target="#convertModal" class="flex-grow-1 flex-md-grow-0 btn btn-danger text-uppercase"><h4 class="m-1">Calculate!</h4></button>
                    </div>
                  </div>
                </div>
                <div class="row justify-content-center mt-4">
                  <span class="text-fine-print">Hand-crafted by <a href="https://www.twitter.com/daffodilistic" target="_blank">@daffodilistic</a></span>
                </div>
              </div>
          </div>
        </div>
      </div>
    </div>
    <div class="modal fade" id="convertModal" tabindex="-1" role="dialog">
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h3 class="text-raleway modal-title"><b>Conversion Results</b></h3>
            <button type="button" class="close" data-dismiss="modal">
              <span>&times;</span>
            </button>
          </div>
          <div class="text-raleway modal-body">
            <div class="text-center container">
              Your character is now:
              <h4 class="text-primary"><b>Level {{ charData.level }}</b></h4>
              <h5><b>Current Advancement Checkpoints*</b></h5>
              <div class="row justify-content-center text-success align-items-center">
                <div class="col-5 text-center">Normal Advancement</div>
                <div class="col-5"><b>{{ charData.advancementPoints.normal }}</b></div>
              </div>
              <div class="row justify-content-center text-danger align-items-center">
                <div class="col-5 text-center">Slow Advancement</div>
                <div class="col-5"><b>{{ charData.advancementPoints.slow }}</b></div>
              </div>
              <div class="row justify-content-center align-items-center mt-4">
                <span class="text-fine-print">*NOTE:&nbsp;<a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/round" target="_blank">Default rounding</a>&nbsp;is used</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data: function() {
    return {
      charXP: "",
      charData: {
        level: 0,
        advancementPoints: {}
      }
    };
  },
  methods: {
    getAP: function() {
      // TODO: Tweak/turn off eslint, this is getting ridiculous
      var charExperience = isNaN(parseInt(this.charXP))
        ? 0
        : parseInt(this.charXP);
      if (charExperience < 0) {
        charExperience = 0;
      }


      // From MPMB: https://github.com/morepurplemorebetter/MPMBs-Character-Record-Sheet/blob/master/_variables/Lists.js
      var experiencePointsList = [
        0,
        300,
        900,
        2700,
        6500,
        14000,
        23000,
        34000,
        48000,
        64000,
        85000,
        100000,
        120000,
        140000,
        165000,
        195000,
        225000,
        265000,
        305000,
        355000
      ];
      //var levels = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20];
      var tiers = [
        { levelLimit: 4, checkPoints: 4 },
        { levelLimit: 10, checkPoints: 8 },
        { levelLimit: 16, checkPoints: 8 },
        { levelLimit: 20, checkPoints: 8 }
      ];

      var nextCharLevel = 0;
      var experienceRemainder = 0;
      var currentTier = 0;
      experiencePointsList.forEach(amount => {
        if (charExperience >= amount) {
          nextCharLevel++;
          experienceRemainder = charExperience - amount;
        }

        if (nextCharLevel + 1 > tiers[currentTier].levelLimit) {
          currentTier++;
        }
      });

      if (experienceRemainder >= experiencePointsList[19]) {
        // Capped!
        //console.log("Char is L20 and beyond!");
        var returnValue = {
          level: 20,
          advancementPoints: {
            normal: "You win D&D!",
            slow: "You Win D&D!"
          }
        };
      } else {
        var shortfall =
          experienceRemainder /
          (experiencePointsList[nextCharLevel] -
            experiencePointsList[nextCharLevel - 1]);
        var advancementPoints = Math.round(
          shortfall * tiers[currentTier].checkPoints
        );

        // console.log("Char level is " + nextCharLevel);
        // console.log("Char Tier is " + (currentTier + 1));
        // console.log("AP is " + advancementPoints);
        // console.log("Next level XP is " + experiencePointsList[(nextCharLevel)]);
        // console.log("Remainder is " + experienceRemainder);
        // console.log("Shortfall % is " + shortfall);
        // console.log(Utilities.formatString('Level %d, %f AP',nextCharLevel, advancementPoints));

        var returnValue = {
          level: nextCharLevel,
          advancementPoints: {
            normal: advancementPoints,
            slow: advancementPoints / 2.0
          }
        };
      }

      this.charData = returnValue;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.text-fine-print {
  font-size: 0.75em;
}

.text-raleway {
  font-family: "Raleway", sans-serif;
  /* color: #fff; */
}

.bg-translucent {
  background: rgba(22, 22, 22, 0.8);
  border-radius: 5px;
}

.header {
  font-family: "Noto Sans", sans-serif;
}
</style>
