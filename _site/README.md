# AIPs
Ampleforth Improvement Proposals (AIPs) describe standards for the Ampleforth platform, including core protocol specifications, client APIs, and contract standards.

WIP: A browsable version of all current and draft AIPs can be found on [the official AIP site](https://aips.ampleforth.org/).

# Contributing

 1. Review [AIP-1](AIPs/aip-1.md).
 2. Fork the repository by clicking "Fork" in the top right.
 3. Add your AIP to your fork of the repository. There is a [template AIP here](aip-X.md).
 4. Submit a Pull Request to Ampleforth's [AIPs repository](https://github.com/ampleforth/AIPs).

Your first PR should be a first draft of the final AIP. It must meet the formatting criteria enforced by the build (largely, correct metadata in the header). An editor will manually review the first PR for a new AIP and assign it a number before merging it. Make sure you include a `discussions-to` header with the URL to a new thread on [ampltalk.org](https://ampltalk.org) where people can discuss the AIP as a whole.

If your AIP requires images, the image files should be included in a subdirectory of the `assets` folder for that AIP as follow: `assets/aip-X` (for aip **X**). When linking to an image in the AIP, use relative links such as `../assets/aip-X/image.png`.

When you believe your AIP is mature and ready to progress past the WIP phase, you should ask to have your issue added to the next governance call where it can be discussed for inclusion in a future platform upgrade. If the community agrees to include it, the AIP editors will update the state of your AIP to 'Approved'.

# AIP Statuses

* **WIP** - a AIP that is still being developed.
* **Proposed** - a AIP that is ready to be reviewed in a governance call.
* **Approved** - a AIP that has been accepted for implementation by the Ampleforth community.
* **Implemented** - a AIP that has been released to mainnet.
* **Rejected** - a AIP that has been rejected.


# Validation

AIPs must pass some validation tests.  The AIP repository ensures this by running tests using [html-proofer](https://rubygems.org/gems/html-proofer) and [aip_validator](https://rubygems.org/gems/aip_validator).

It is possible to run the AIP validator locally:
```
gem install aip_validator
aip_validator <INPUT_FILES>
```
