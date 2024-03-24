---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.1
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

+++ {"editable": true, "slideshow": {"slide_type": ""}}

# Python Access to LSEG Workspace

+++ {"editable": true, "slideshow": {"slide_type": ""}}

(references)=
## References

- [Quick Start](https://developers.lseg.com/en/api-catalog/eikon/eikon-data-api/quick-start)
- [Tutorials](https://developers.lseg.com/en/api-catalog/eikon/eikon-data-api/tutorials)

+++ {"editable": true, "slideshow": {"slide_type": ""}}

## Steps

1. Download the desktop app from [here](https://cdn.refinitiv.com/Apps/ProductDownloadPage/1.1.13/)
2. Follow the instructions from [here](https://developers.lseg.com/en/api-catalog/eikon/eikon-data-api/quick-start) to obtain the API Key (select Eikon Data API, Side by Side API and EDP API)
3. On command line, invoke: <code>pip install eikon</code>4. Start the desktop app and have it running in the background
5. Invoke the follwing commands in Python to check that API access works: <code>import eikon as ek; 
ek.set_app_key('8e5a3xxxxxxxxxxxxxxxxxxxxxxxxxxxx21b031c</code>')The alternative to Steps 3 to 5 is via Jupyter Notebook/Lab as follows:



```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
!pip install eikon
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
import eikon as ek
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
tags: [remove-input]
---
ek.set_app_key('edafd42def94453a8ee57abdd18f7c797d392485')
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
#ek.set_app_key('8e5a3xxxxxxxxxxxxxxxxxxxxxxxxxxxx21b031c')
```

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
df = ek.get_timeseries(["MSFT.O"],
                       start_date="2016-01-01",
                       end_date="2016-01-10")
df
```

+++ {"editable": true, "slideshow": {"slide_type": ""}}

Further instructions may be found in the Tutorials link under {ref}`references` above.
