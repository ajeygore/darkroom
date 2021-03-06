# Image Processor for Darkroom

#### About
This module holds the logic to process images. It is used by the Darkroom [Application Server](https://github.com/gojek/darkroom).  
You may implement the `Processor` interface to gain custom functionality while still keeping other Darkroom functionality.

#### Interface
```go
type Processor interface {
	Crop(img image.Image, width, height int, point CropPoint) image.Image
	Decode(data []byte) (image.Image, string, error)
	Encode(img image.Image, format string) ([]byte, error)
	GrayScale(img image.Image) image.Image
	Resize(img image.Image, width, height int) image.Image
	Watermark(base []byte, overlay []byte, opacity uint8) ([]byte, error)
	Flip(image image.Image, mode string) image.Image
	Rotate(image image.Image, angle float64) image.Image
	FixOrientation(image image.Image, orientation int) image.Image
}
```
Any `struct` implementing the above interface can be used with Darkroom.

#### Example

```go
bp := NewBildProcessor()
srcImgData, _ := ioutil.ReadFile("test.png")
srcImg, _, _ := bp.Decode(srcImgData)
outputImg, err := bp.Resize(srcImg, 500, 500)
outputImgData, _ := bp.Encode(outputImg, "png")
_ = ioutil.WriteFile("output.png", outputImgData, 0644)
```