<template>
    <article class="col mb-3">
        <div class="card card--group">
            <div class="card-header">Group {{ group.getDisplayName() }}</div>
            <table class="table-bordered" v-if="withstanding">
                <thead>
                <tr>
                    <th scope="col">&nbsp;</th>
                    <th scope="col" class="text-center"><abbr title="Played">Pld</abbr></th>
                    <th scope="col" class="text-center"><abbr title="Wins - Draws - Lost">W-D-L</abbr></th>
                    <th scope="col" class="text-center"><abbr title="Goals">G</abbr></th>
                    <th scope="col" class="text-center"><abbr title="Goal difference">GD</abbr></th>
                    <th scope="col" class="text-center"><abbr title="Points">Pts</abbr></th>
                </tr>
                </thead>
                <tbody>
                <tr v-for="(standing, index) in group.getStandings()" :key="index" :class="standingpositon(index)">
                    <td><teamname :team="standing.getTeam()"></teamname></td>
                    <td class="text-center">{{ standing.getPlayed() }}</td>
                    <td class="text-center text-nowrap">{{ standing.getWins() }} - {{ standing.getDraws() }} - {{ standing.getLosts() }}</td>
                    <td class="text-center text-nowrap">{{ standing.getGoalsFor() }} - {{ standing.getGoalsAgainst() }}</td>
                    <td class="text-center">{{ standing.getGoalsDifference() }}</td>
                    <td class="text-center">{{ standing.getPoints() }}</td>
                </tr>
                </tbody>
            </table>
            <div class="card-footer p-0">
                <table class="table-groups">
                    <tbody :class="[show ? 'tbody--open' : 'tbody--close']">
                        <match v-for="(match, index) in group.getMatches()" :id="group.getName()" :game="match" :gametype="'groups'" :key="index"></match>
                    </tbody>
                </table>
                <button v-if="withstanding" @click="isShow = !isShow" type="button" :class="[show ? 'close--close' : 'close--plus']" class="close"><span>&times;</span></button>
            </div>
        </div>
    </article>
</template>

<script>
    import GroupModel from '../Model/group';
    import Teamname from './Teamname.vue';
    import Match from './Match.vue';
    export default {
        data() {
            return {
                isShow: false,
            };
        },
        methods: {
            standingpositon(index) {
                return index === 0 ? 'table-success' : index === 1 ? 'table-info' : '';
            },
            match_prognostics(id){
                return this.prognostics.find(function (obj) { return obj.match_id === id; });
            }
        },
        computed: {
            show() {
                if (!this.withstanding) {
                    return true;
                }
                return this.isShow;
            },
            
        },
        props: {
            withstanding: {
                type: Boolean,
                required: false,
                default: true,
            },
            group: {
                type: GroupModel,
                required: true,
            }
        },
        components: {
            Teamname,
            Match,
        },
    };
</script>
