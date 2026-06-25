# Milestone 1 FAQ

## Scroll Data Persists Between Story/Feed and Followers/Following

This isn't a super big issue right now, but it will be for future phases when we add actual data.

Short explanation: React doesn't see a difference between our Scroller components that remove duplicate code. We need to use a key to tell React that they are different to force React to rerender these components. These videos further explain this.

- [CS 340 M1 Part 2 Demo](https://www.youtube.com/watch?v=MeXHcXQpUCY&t=1068s)

- [CS 340 M2B Component Refactoring Overview](https://youtu.be/kEsC0rbVpPY?si=bDHWPdo6n-s_PdJe&t=392)

## Clicking on a user should navigate to that user's page

Clicking on a user should preserve the current feature URL. This feature URL would be `/feed`, `/story`, `/followers`, or `/followees`. If you click on a user and go to a different feature URL, then you likely have it hardcoded in your StatusItem or UserItem
