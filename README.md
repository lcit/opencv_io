# opencv_io

C++ - Some nice utils to handle input and output (display or recording) streams using OpenCV.

### Prerequisites

OpenCV

### Example usage

```C++
int main(int argc, char* argv[]) {

    InputStream is(argv[1]);
    if(!is.open()) {
        std::cout << "Unable to open input stream\n";
        exit(-1);
    }
    
    std::cout   << "Total number of frames : " << is.n_frames() 
                << " Width : " << is.width() 
                << " Height : " << is.height() << "\n";

    DisplayStream ds(argv[2], 400, 600, 100, 100);
    ds.open();
    ds.add_trackbar("trackbar 1", 0, 10);
    ds.set_trackbar_value("trackbar 1", 5);
    ds.add_trackbar("trackbar 2", 20, 100);
    ds.set_trackbar_value("trackbar 2", 50);
    
    // if you use a custom size, the frame a firstly resized then saved
    //RecordStream rs(argv[3], is.fps(), 300, 200);
    RecordStream rs(argv[3], is.fps(), is.width(), is.height());
    if(!rs.open()) {
        std::cout << "Unable to open output stream (recording)\n";
        exit(-1);
    }
    
    cv::Mat frame;
 
    while(1){
        std::cout   << "Frame per seconds : " << is.fps() 
                    << " Frame index : " << is.frame_number() 
                    << " Timestamp : " << is.timestamp() 
                    << " Trackbar 1 : " << ds.get_trackbar_value("trackbar 1") 
                    << " Trackbar 2 : " << ds.get_trackbar_value("trackbar 2") 
                    << "\n";
        
        // retrieve a frame
        if(!is.next_frame(frame))
            break; // exit when video is terminated
        
        // display the frame
        ds.next_frame(frame);
        
        // save the frame
        rs.next_frame(frame);
    }
    return 0;
}
```

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

