Blue Green Deployments
======================

The master is blue, the branch is green.

Deploy from OSEv3.

new project and blue app from master
====================================

    oc new-project bluegreen --display-name="Blue Green" --description='Blue Green Deployments'
    oc new-app https://github.com/eformat/bluegreen#master --name=blue --strategy=sti

expose bluegreen service (using blue)
=====================================

    oc expose service blue --name=bluegreen --hostname=bluegreen.cloudapps.ose.eformat.co.nz

green app deploy
================

    oc new-app https://github.com/eformat/bluegreen#green --name=green --strategy=sti

switch services to green
========================

    oc get route/bluegreen -o yaml | sed -e 's/name: blue$/name: green/' | oc replace -f -

and back again
==============

    oc get route/bluegreen -o yaml | sed -e 's/name: green$/name: blue/' | oc replace -f -

test application here
=====================

    http://bluegreen.cloudapps.ose.eformat.co.nz/
