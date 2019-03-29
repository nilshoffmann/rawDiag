# A recipe on howto compile and run a commandline program using *RawFileReader from Thermo Fisher Scientific*


## Install

- login; on your Linux Docker server

- Get *The New RawFileReader from Thermo Fisher Scientific* from http://planetorbitrap.com/rawfilereader

- Place the rawfile reader nuget package in the 'ThermoRawFileReader' directory (ThermoFisher.CommonCore.RawFileReader.4.0.26.nupkg)

## Build Docker image

```bash 
docker build -t $USER/rawdiag:v1  .
```

## Run the Docker image

```bash
docker run --rm -p28288:3838 $USER/rawdiag:v1
```

Open your browser at 

http://localhost:28288/rawdiag

to see the program in action.

## Testing

In the rawDiag web-application, select *Type of data source* -> *filesystem* and upload a RAW file via the *Upload RAW files* dialog.
Select the RAW file by name to populate the various charts in the tabset on the right side of the UI.

