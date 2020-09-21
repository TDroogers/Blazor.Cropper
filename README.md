# Blazor.Cropper
A blazor library provide a component to crop image  
![](imgs/base.gif)=>  
![](imgs/1.gif) ![](imgs/2.gif) ![](imgs/3.gif)

It is:
- almost full c#(with only 3 lines of js)
- mobile compatible
- lighweight
- support proportion
- **GIF crop support**(only for files smaller than 1mb)


For a long time, crop image in blazor bother me a lot. That's why I tried to implement a cropper in blazor.

## Usage
to use it, you should first paste following code into your index.html:  
```html
<script src="_content/Chronos.Blazor.Cropper/CropHelper.js"></script>
```
Then, you can install our [nuget pkg](https://www.nuget.org/packages/Chronos.Blazor.Cropper) and use it like follow:
```razor
@page "/cropper"

<h1>Cropper</h1>
<InputFile OnChange="OnInputFileChange"></InputFile>
@if (!string.IsNullOrEmpty(imgUrl))
{
    <center>
        <h2>Crop Result:</h2>
        <img src="@imgUrl"/>
    </center>   
}
@if (file!=null)
{
    <Cropper ImageFile="file" OnCrop="@OnCropedAsync"></Cropper>
}
@code {
    IBrowserFile file;
    string imgUrl = "";
    void OnInputFileChange(InputFileChangeEventArgs args)
    {
        file = args.File;
    }
    async Task OnCropedAsync(Stream stream)
    {
        var bytes = new byte[stream.Length];
        await stream.ReadAsync(bytes,0,(int)stream.Length);
        var format = "image/png";
        imgUrl = $"data:{format};base64,{Convert.ToBase64String(bytes)}";
    }
}

```
For more details, see [the sample project](CropperSample).  
To build it, simply clone it and run it in visual studio. The running result should be like this:  
![](2020-09-20-22-00-04.png)  
## Note
In many cases, I found It's really slow to convert image data to base64 format and set it as img src(many times slower than image crop process). So I stronly recommend you to avoid doing this in blazor.

