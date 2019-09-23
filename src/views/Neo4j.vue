<template>
    <div id="neo4j">
        <div class="container">
            <h3>Neo4j</h3>
            <div id="neo4jd3">
            </div>
        </div>
    </div> 
</template>


<script>
/* eslint-disable */
import NEO4JD3 from 'neo4jd3'

export default {
    name: 'Neo4j',
    data(){
        return {
            data: null,
            url: "http://swarm1:7474/db/data/transaction/commit",
            neo4j: null
        }
    },
    methods: {
        fetchData: function (){
            let query = {
                "statements": [
                    {
                        "statement": "MATCH p=()-[r:ACTED_IN]->() RETURN p LIMIT 5",
                        "resultDataContents": ["graph"]
                    }
                ]
            }
            const options = {
                'headers': {
                    'Authorization': 'Basic bmVvNGo6dGVzdA==',
                    'Content-Type': 'application/json'
                }
            }
            //$.ajax({
            //    url: this.url,
            //    headers: options.headers,
            //    method: 'POST',
            //    dataType: 'json',
            //    data: JSON.stringify(query),
            //    success: function (data){
            //        console.log("from jquery", data)
            //    }
            //})
            this.axios.post(this.url, query, options).then((rs) => {
                this.data = rs.data
                console.log("response data", rs)
                //this.neo4j = new NEO4JD3('#neo4jd3', {
                let self = this
                this.neo4j = new Neo4jd3('#neo4jd3', {
                //this.neo4j = new NEO4JD3('#neo4jd3', {
                    minCollision: 60,
                    neo4jData: this.data,
                    nodeRadius: 25,
                    onNodeClick: function (node){
                        let secondQuery = {
                            "statements": [
                                {
                                    "statement": "MATCH(n)<-[r]->(c) where ID(n)    = " + node.id + " return n,c,r LIMIT 3",
                                    "resultDataContents": ["graph"]
                                }
                            ]
                        }
                        self.axios.post(self.url, secondQuery, options).then((rs) => {
                            self.neo4j.updateWithNeo4jData(rs.data)
                        })
                    }
                })
            })
        }
    },
    mounted(){
        this.fetchData()
        console.log("here> ", Neo4jd3)
    },
    watch: {
    }
}
</script>


<style scoped>
 .neo4jd3 {
    height: 600px;
    overflow: hidden;
}

</style>