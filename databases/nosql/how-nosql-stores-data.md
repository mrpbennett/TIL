# How NoSQL stores data ELI5 version

In a NoSQL database, data is stored in a way that doesn't follow the traditional
table structure like you might see in a spreadsheet. Instead, it uses a more
flexible approach that allows different types of data to be stored together
without a fixed schema (or set of rules).

So, let's take our toy box (NoSQL database) and look at how each type of toy
(data) is stored:

1. **Key-Value Store**: Imagine you have a bag with toy cars (values) and each
   car has a unique label (key) like "Red Car," "Blue Car," and so on. When you
   need to find a specific car, you just look at the label (key) and grab the
   corresponding toy car (value).

2. **Document Store**: In this case, let's say you have a set of LEGO building
   blocks with instructions. Each set of LEGO blocks and its instructions are
   grouped together in separate bags, with each bag having a label describing
   what's inside. So, you might have a bag labeled "Pirate Ship" that contains
   all the LEGO pieces and instructions to build a pirate ship.

3. **Column Family Store**: Imagine you have a bunch of puzzle pieces with
   different shapes and colors. In a column family store, you organize the
   puzzle pieces based on their shape or color. So, you might have one section
   for all the blue puzzle pieces, another section for the red ones, and so on.

4. **Graph Database**: Think of a graph database as a bunch of puzzle pieces
   connected together. Each puzzle piece represents a unique piece of data, and
   they are connected like a jigsaw puzzle. This allows you to easily find
   related pieces of information.

In each of these cases, the NoSQL database allows you to store and retrieve data
in a more flexible way compared to traditional relational databases. It's like
having a toy box that can organize and store all kinds of toys without needing
fixed compartments or a specific arrangement. This flexibility makes NoSQL
databases useful for handling different types of data and accommodating changes
over time.
