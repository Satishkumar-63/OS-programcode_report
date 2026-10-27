#include <stdio.h>

int main() {
    int f, p;
    
    printf("Enter the number of frames: ");
    scanf("%d", &f);
    
    printf("Enter the number of pages: ");
    scanf("%d", &p);
    
    int pages[p];
    printf("Enter the reference string (page numbers):\n");
    for(int i = 0; i < p; i++) {
        scanf("%d", &pages[i]);
    }
    
    int frames[f], time[f];
    for(int i = 0; i < f; i++) {
        frames[i] = -1;
        time[i] = 0;
    }
    
    int faults = 0, counter = 0;
    
    printf("\nPage Reference\tFrames\n");
    
    for(int i = 0; i < p; i++) {
        int flag1 = 0, flag2 = 0;
        
        // Check if page is already present in frames (Hit)
        for(int j = 0; j < f; j++) {
            if(frames[j] == pages[i]) {
                counter++;
                time[j] = counter;
                flag1 = flag2 = 1;
                break;
            }
        }
        
        // If not present, find an empty frame (Miss)
        if(flag1 == 0) {
            for(int j = 0; j < f; j++) {
                if(frames[j] == -1) {
                    counter++;
                    faults++;
                    frames[j] = pages[i];
                    time[j] = counter;
                    flag2 = 1;
                    break;
                }
            }
        }
        
        // If frames are full, replace the Least Recently Used page
        if(flag2 == 0) {
            int pos = 0, min = time[0];
            for(int j = 1; j < f; j++) {
                if(time[j] < min) {
                    min = time[j];
                    pos = j;
                }
            }
            counter++;
            faults++;
            frames[pos] = pages[i];
            time[pos] = counter;
        }
        
        // Print current state of frames
        printf("Page %d \t\t", pages[i]);
        for(int j = 0; j < f; j++) {
            if(frames[j] != -1)
                printf("%d ", frames[j]);
            else
                printf("- ");
        }
        printf("\n");
    }
    
    printf("\nTotal Page Faults = %d\n", faults);
    return 0;
}
