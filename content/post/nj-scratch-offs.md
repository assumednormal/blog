---
title: "NJ Scratch Offs"
date: 2020-01-27
tags: []
draft: true
mathjax: false
disableComments: false
---

NJ is one of only a few states that reports the number of tickets remaining at each prize level for each scratch off game each day. Assuming that the proportion of $0 tickets remaining is the same as the second most frequent prize level and that a prize of a free ticket is immediately exchanged for a new ticket, we can calculate the expected value of the next ticket purchased.

Click the button below to retrieve up-to-date information on active scratch off games.

{{< rawhtml >}}
<div>
<button type="button" id="updBtn">Update</button>
</div>


<div id="results">
</div>


<script>
    document.getElementById("updBtn").addEventListener("click", function() {
        var req = new XMLHttpRequest();
        req.open("GET", "https://cors-anywhere.herokuapp.com/https://www.njlottery.com/api/v1/instant-games/games?size=1000");
        req.onreadystatechange = function() {
            if (req.readyState === XMLHttpRequest.DONE && req.status === 200) {
                var games = JSON.parse(req.responseText).games;
                var activeGames = games.filter(game => game.validationStatus == "ACTIVE");
                var enrichedGames = activeGames.map(function(game) {
                    game.zeroPayoutPrinted = game.totalTicketsPrinted - game.prizeTiers.reduce(function(tickets, prizeTier) {
                        return tickets + prizeTier.winningTickets;
                    }, 0);
                    var tier1Prize = game.prizeTiers.filter(prizeTier => prizeTier.tierNumber == 1)[0];
                    game.zeroPayoutRemaining = Math.round(game.zeroPayoutPrinted * (tier1Prize.winningTickets - tier1Prize.paidTickets) / tier1Prize.winningTickets);
                    game.freeTicketsRemaining = game.prizeTiers.filter(prizeTier => prizeTier.prizeDescription.startsWith("FREE")).reduce(function(tickets, prizeTier) {
                        return tickets + prizeTier.winningTickets - prizeTier.paidTickets;
                    }, 0);
                    game.nonFreeTicketsRemaining = game.zeroPayoutRemaining + game.prizeTiers.filter(prizeTier => !prizeTier.prizeDescription.startsWith("FREE")).reduce(function(tickets, prizeTier) {
                        return tickets + prizeTier.winningTickets - prizeTier.paidTickets;
                    }, 0);
                    game.expectedValue = Math.round(game.prizeTiers.filter(prizeTier => !prizeTier.prizeDescription.startsWith("FREE")).reduce(function(ev, prizeTier) {
                        return ev + (prizeTier.winningTickets - prizeTier.paidTickets) / game.nonFreeTicketsRemaining * prizeTier.prizeAmount;
                    }, 0)) / 100;
                    return game;
                });
                var tableRows = "<table class='table table-striped'><tr><th>Game ID</th><th>Game Name</th><th>Ticket Cost</th><th>Expected Value</th></tr>" +
                    enrichedGames.sort(function(game1, game2) {
                        if ((game1.expectedValue - game1.ticketPrice / 100) > (game2.expectedValue - game2.ticketPrice / 100)) {
                            return -1;
                        }
                        if ((game1.expectedValue - game1.ticketPrice / 100) < (game2.expectedValue - game2.ticketPrice / 100)) {
                            return 1;
                        }
                        return 0;
                    }).map(function(game) {
                    return "<tr><td>" +
                        game.gameId +
                        "</td><td>" +
                        game.gameName +
                        "</td><td>" +
                        game.ticketPrice / 100 +
                        "</td><td>" +
                        game.expectedValue +
                        "</td></tr>";
                }).join("") + "</table>";
                document.getElementById("results").innerHTML = tableRows;
            }
        };
        req.send();
    });
</script>
{{< /rawhtml >}}