<details>
```mermaid
flowchart TD

style A fill:#f2e6ff,stroke:#663399,stroke-width:2px

style B1 fill:#f2e6ff,stroke:#663399,stroke-width:2px

style B2 fill:#f2e6ff,stroke:#663399,stroke-width:2px

style B3 fill:#f2e6ff,stroke:#663399,stroke-width:2px

style B4 fill:#f2e6ff,stroke:#663399,stroke-width:2px

style C fill:#f2e6ff,stroke:#663399,stroke-width:2px

  

A[Start Data Preprocessing] --> B1[Load Raw Data<br>file_path: raw_dataset]

B1 -->|Loads data from Excel file| B2[Slice Data<br>upper_cut: 2500, lower_cut: 300]

B2 -->|Slices data based on upper and lower thresholds| B3[Baseline Removal]

B3 -->|Removes baseline using ZhangFit| B4[Normalize Data]

B4 --> C[End of Initial Preprocessing Steps]
```
</details>




```mermaid
flowchart TD

style B5 fill:#f2e6ff,stroke:#663399,stroke-width:2px

style B6 fill:#f2e6ff,stroke:#663399,stroke-width:2px

style B7 fill:#f2e6ff,stroke:#663399,stroke-width:2px

style B8 fill:#f2e6ff,stroke:#663399,stroke-width:2px

style C fill:#f2e6ff,stroke:#663399,stroke-width:2px

style D1 fill:#f2e6ff,stroke:#663399,stroke-width:2px

style D2 fill:#f2e6ff,stroke:#663399,stroke-width:2px

style D3 fill:#f2e6ff,stroke:#663399,stroke-width:2px

style E fill:#f2e6ff,stroke:#663399,stroke-width:2px

style F fill:#f2e6ff,stroke:#663399,stroke-width:2px

  

B5[Find Peaks<br>prominence: 0.02, m: 10] -->|Identifies peaks based on prominence and m| B6[Cluster Peaks<br>cluster_threshold: 20]

B6 -->|Clusters peaks based on threshold| B7{Size of Clusters < m?}

B7 -->|Yes| B5

B7 -->|No| B8[Max Pooling<br>Selects max y-value in each cluster]

B8 --> C[End Data Preprocessing]

  

%% DataFeeder steps

C -->|Processes files in folder_path| D1[Generate Xs<br>folder_path: raw_dataset]

D1 -->|Generates labels based on file names| D2[Generate Ys]

D2 -->|Combines X and Y| D3[Combine Data and Labels]

D3 -->|Saves to save_path| E[Save Processed Data<br>save_path: processed_data/saved_processed.json]

E --> F[End DataFeeding Process]
```



```mermaid
flowchart TD

A[[Input Layer: Shape None, shape_0, shape_1]]

B[[Conv1D: Filters 200, Kernel Size 2, Activation tanh]]

C[[MaxPooling1D: Pool Size 2]]

D[[BatchNormalization]]

E[[Conv1D: Filters 100, Kernel Size 2, Activation tanh]]

F[[MaxPooling1D: Pool Size 2]]

G[[BatchNormalization]]

H[[Flatten Layer]]

I[[Dense: Units 1024, Activation tanh, L2 0.01]]

J[[Dense: Units 128, Activation tanh, L2 0.01]]

K[[Dense: Units 4, Activation linear, L2 0.01]]

L[[Output Layer]]

  

A --> B --> C --> D --> E --> F --> G --> H --> I --> J --> K --> L

  

style A fill:#f2e6ff,stroke:#663399,stroke-width:2px

style B fill:#d1c4e9,stroke:#663399,stroke-width:2px

style C fill:#d1c4e9,stroke:#663399,stroke-width:2px

style D fill:#d1c4e9,stroke:#663399,stroke-width:2px

style E fill:#d1c4e9,stroke:#663399,stroke-width:2px

style F fill:#d1c4e9,stroke:#663399,stroke-width:2px

style G fill:#d1c4e9,stroke:#663399,stroke-width:2px

style H fill:#e8eaf6,stroke:#663399,stroke-width:2px

style I fill:#c5cae9,stroke:#663399,stroke-width:2px

style J fill:#c5cae9,stroke:#663399,stroke-width:2px

style K fill:#c5cae9,stroke:#663399,stroke-width:2px

style L fill:#f2e6ff,stroke:#663399,stroke-width:2px
```

Data preprocessing pipeline


![[Pasted image 20240811121742.png]]

![[Pasted image 20240811122546.png]]



![[Pasted image 20240811123445.png]]




![[Pasted image 20240811124318.png]]


![[Pasted image 20240811131502.png]]