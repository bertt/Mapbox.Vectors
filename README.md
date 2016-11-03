
# mapbox-vector-tile-cs 

[![NuGet Status](http://img.shields.io/nuget/v/mapbox-vector-tile.svg?style=flat)](https://www.nuget.org/packages/mapbox-vector-tile/)

A .NET library for decoding a Mapbox vector tile into a collection of GeoJSON FeatureCollection objects.

For each layer there is one GeoJSON FeatureCollection. Code is tested using the Mapbox tests from

https://github.com/mapbox/vector-tile-js

Dependencies: GeoJSON.NET, JSON.NET, protobuf-net

###Get it from NuGet 
`
PM> Install-Package mapbox-vector-tile
`

https://www.nuget.org/packages/mapbox-vector-tile

NuGet package contains PCL: Profile 111 (.NET Framework 4.5, Windows 8.0, Windows Phone 8.1, Xamarin.Android, Xamarin.iOS)

# Usage

```cs
const string vtfile = "vectortile.pbf";
using (var stream = File.OpenRead(vtfile))
{
  // parameters: tileColumn = 67317, tileRow = 43082, tileLevel = 17 
  var layerInfos = VectorTileParser.Parse(stream);
  var fc = layerInfos[0].ToGeoJSON(67317,43082,17);
  Assert.IsTrue(fc.Features.Count == 47);
  Assert.IsTrue(fc.Features[0].Geometry.Type == GeoJSONObjectType.Polygon);
  Assert.IsTrue(fc.Features[0].Properties.Count==2);
}
```

See also https://github.com/bertt/mapbox-vector-tile-cs/blob/master/tests/mapbox.vector.tile.tests/TileParserTests.cs for working examples

Tip: If you use this library with vector tiles loading from a webserver, you could run into the following exception: 
'ProtoBuf.ProtoException: Invalid wire-type; this usually means you have over-written a file without truncating or setting the length'
Probably you need to check the GZip compression, see also TileParserTests.cs for an example.

# History
2016-11-03: Release 3.1

Changes: Add support for polygon inner- and outerrings

2016-10-31: Release 3.0

Changes: Add support for multi-geometries 

2015-07-08: Release 2.0 

2015-07-07: Release 1.0 

# Benchmark test
Test performed with Mapbox vector tile '14-8801-5371.vector.pbf'
```
Host Process Environment Information:
BenchmarkDotNet.Core=v0.9.9.0
OS=Microsoft Windows NT 6.2.9200.0
Processor=Intel(R) Core(TM) i7-6820HQ CPU 2.70GHz, ProcessorCount=8
Frequency=2648440 ticks, Resolution=377.5808 ns, Timer=TSC
CLR=MS.NET 4.0.30319.42000, Arch=32-bit RELEASE
GC=Concurrent Workstation
JitModules=clrjit-v4.6.1586.0

Type=ParsingBenchmark  Mode=Throughput
                    Method |    Median |    StdDev |
-------------------------- |---------- |---------- |
 ParseVectorTileFromStream | 1.7147 us | 0.0196 us |

# Projects that use mapbox-vector-tile-cs

* OSMSharp VectorTileToBitmapRenderer Mapzen vector tile layer demo in 
https://github.com/OsmSharp/VectorTileToBitmapRenderer

* BruTile BruTile.Samples.VectorTileToBitmap
https://github.com/BruTile/BruTile/tree/master/Samples/BruTile.Samples.VectorTileToBitmap

* longlostbro/MapsDevelopmentApplication/RadControls/
https://github.com/longlostbro/MapsDevelopmentApplication/tree/master/RadControls

* ArcBruTile/ArcBruTile
https://github.com/ArcBruTile/ArcBruTile

* Mapbox vector tile uploader. Upload vector tile and display on Esri 3D Javascript map http://mapboxvectortileuploaderthingymajig.azurewebsites.net/

* indirasam/StreetHarassmentReporting/CartMap
https://github.com/indirasam/StreetHarassmentReporting/tree/indira/CartMap

