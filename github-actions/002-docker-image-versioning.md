# Versioning of Docker Images
Date: 2022-02-21
<br><br>

## Status

Proposed
<br><br>

**Note:** This invalidates [001-publish-SNAPSHOT-docker-images-github-actions](https://github.com/Bahmni/bahmni-decision-records/blob/main/github-actions/001-publish-SNAPSHOT-docker-images-github-actions.md)

## Context
Now that we have some of the components of Bahmni dockerized and we are using Github Actions to build and publish docker images to Dockerhub we need a versioning strategy for the docker images.
<br><br>

## Descision
- Normally the artifacts of every build will be tagged with the commit SHA that will allow to get to the exact point of the code from which the artifact was generated. But in case of Bahmni, this is hard to be followed because for an image/artifact we have a couple of dependent repositories from where the build would be triggered. So the versioning template for Bahmni would look like 

    > `bahmni/<imageName>:<bahmniVersion>-<githubRunNumber>`

    Example: bahmni/openelis:0.94-1

- Whenever there is a push to `master` or `main` branch, Github Actions will build an image tagged with `githubRunNumber` and also as `latest`, so that latest contains the artifact of code that is in `master` or `main` for a given point in time.

- Now, whenever we foresee a release for Bahmni, we create a release branch of the format `release/<bahmniVersion>`, and whenever there is a push to the release branch the workflow will create release candidate images and tag the images in the format
  > `bahmni/<imageName>:<bahmniVersion>-rc`

    Example: bahmni/openelis:0.94-rc

- After the release canditate is well tested and ready for release, then a Github Tag is created and pushed. On push of a tag, then release workflow will tag the release candidate image as version release image of the format
    > `bahmni/<imageName>:<bahmniVersion>`

    Example: bahmni/openelis:0.94
<br><br>

## Consequences
- This versioning strategy will be followed across all docker images of Bahmni. The `latest` can be used for development setup which makes it easy to start with latest code on main branch of the repository. 
- Since the release image publishing happens on push of github tag, proper validation and approvals must be added the workflow before publishing the image on Dockerhub.
<br><br>

## References:
1. [Discussion during PAT call](https://docs.google.com/document/d/13FyHQ3a_32fTFtdW_Ez3BjGRbkPwZ8ztoNhF7wTVUko/edit?usp=sharing)

