#### Then your app use env variables you can set them in values.yaml and import to template using this steps:

##### Put your env variables in helm values.yaml as below:
```yaml
env:          
 VAR_1: "value"
 VAR_2: "value"
 VAR_N: "value"
```

##### Edit the deployment yaml like this:

```yaml
spec:
  containers:
    - name: {{ .Chart.Name }}
      image: "{{ .Values.image.repository }}:{{ .Values.image.version }}"
      imagePullPolicy: {{ .Values.image.pullPolicy }}
      env:          
      {{- range $key, $val := .Values.env }}
        - name: {{ $key }}
          value: {{ $val | quote }}
      {{- end }}
```
