# Testing Your M4B Performance

A good way to test that your performance is enough for the M4B passoff is to verify that your feed table can be populated with 10k new items
within two minutes. follow the instructions below to test this.

## Set up

1. Create a Tweeter user with at least 10,000 followers (your "10k user").
1. Log into DynamoDB and set all your tables' Read Capacity and Write Capacity values to 100 RCUs or WCUs. (You will probably
only need this for your `feeds`, `users`, and `follows` tables, but better safe than sorry.) Note that NONE of your tables should
be set to `On Demand`. Remember to turn these back down afterwards to avoid getting charged by AWS!
1. Post a status as your 10k user; this will warm up your lambdas. Then wait a few minutes before running the test to let your queues empty.

## Testing performance

1. Log into DynamoDB, open your `feeds` table, and click `Get live item count`. Click `Start scan`, and take note of the number it gives you.
This is your "initial count." (Keep this page open; you'll need it again in a bit.)
1. Log into Tweeter as your 10k user.
1. Open a timer or stopwatch for 2 minutes.
1. Post a status, and start the timer at the same time that you click `Submit post`.
1. After about 20 seconds, click `Start scan` again and take note of the number it gives you. This is your "partial count."
1. At exactly the 2-minute mark, click `Start scan` a third time and take note of the number it gives you. This is your "final count."

## Analyzing the results

If everything went correctly:

- Your final count should be 10,000 items larger than your initial count.
- Your partial count should **NOT** be 10,000 items larger than your initial count. (It will likely be, at most, 2 or 3 thousand items larger.)

## Problems?

If your partial count is close to (or the same as) your final count, you are probably relying on burst capacity. (With only 100 WCUs,
it shouldn't be possible to finish in only 20 seconds, even with batch writes.) It doesn't necessarily mean your architecture is wrong,
but it does mean the test wasn't meaningful. Try again soon after the last attempt, and see if you get the same result.

If your final count is not high enough, not all the feed items were posted in time. Check your SQS queues and see if there
are any messages still in flight, and/or scan again a short while later to see if all 10,000 do eventually get added.
