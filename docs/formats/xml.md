---
layout: default
title: XML format
---

# {{ page.title }}


The XML format uses simple XML for returning a result set.   For example:

    https://sandbox.demo.socrata.com/resource/cuqs-ssxu.xml

will return

    <response>
      <row>
        <row _id="1" _uuid="88CA74DC-8F54-469F-8A90-469E111D5D03" _address="http://sandbox.demo.socrata.com/views/cuqs-ssxu/rows/1">
          <name>Aaron, Henry Jacob</name>
          <position>Member</position>
          <agency_name>Social Security Advisory Board</agency_name>
          <agency_website>SSAB (http://www.ssab.gov)</agency_website>
          <nomination_date>2011-02-14T08:00:00</nomination_date>
        </row>
      </row>
    </response>

