## Sequence Diagrams
### Overview
Use these sequence diagrams to see how redbridge use cases are executed "under the hood." Components are listed across the top and bottom, and application flow is from top to bottom. For a zoomed-in view, see the corresponding [communication](../Communication/) diagrams.

For clarity, these sequence diagrams are optimistic and assume the briding process goes smoothly. However, dealing with component failures is crucial for the bridge to function resiliantly. Each sequence diagram has a corresponding *notes* document that includes a table describing how component and communication failures are dealt with for each step of the sequences. We opted to put these in table form rather than use activity diagrams (as we had originally proposed) because we believe it's clearer to show them all together in table form rather than creating many activity diagrams that you would have to flip between.

Each large step of each sequence are shown as grouping boxes around smaller steps, so for a quick overview, you can read the titles of the boxes from top to bottom. Then you can go back and dive into the details.