---
layout: default
title: RDF Format
---

# {{ page.title }}

The RDF format is a little different than many of the other formats, because that dataset (or view) owners are able
to map the output to different semantic web schemas, through changing the metadata on the dataset itself, or the columns
of the dataset.

For example, the following URL

[https://sandbox.demo.socrata.com/resource/nominationsCopy.rdf?$limit=1](https://sandbox.demo.socrata.com/resource/nominationsCopy.rdf?$limit=1)

yields:

    <rdf:RDF>
        <dsbase:_6cpn-3h7n rdf:about="http://sandbox.demo.socrata.com/views/_6cpn-3h7n/rows/1">
            <socrata:rowID>1</socrata:rowID>
            <rdfs:member rdf:resource="http://sandbox.demo.socrata.com/views/_6cpn-3h7n"/>
            <ds:name>Aaron, Henry Jacob</ds:name><ds:position>Member</ds:position>
            <ds:agency_name>Social Security Advisory Board</ds:agency_name>
            <ds:agency_website>SSAB (http://www.ssab.gov)</ds:agency_website>
            <ds:nomination_date>2011-02-14T08:00:00</ds:nomination_date>
        </dsbase:_6cpn-3h7n>
    </rdf:RDF>

To change the formats in this, you can change the values in the metadata for the column.

