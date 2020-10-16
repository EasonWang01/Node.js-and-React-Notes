# Modal

可以用 react-native-modal 模組，但在 native-base 下可能有問題，可以用原生替代。

{% embed url="https://stackoverflow.com/questions/56941252/native-base-and-modal-not-working-in-react-native" %}

## 範例

```javascript
 <View style={{ flex: 1 }}>
   <Modal transparent={true} animationType={"slide"} visible={true}>
                <View
                  style={{
                    flex: 1,
                    justifyContent: "center",
                    alignItems: "center",
                    backgroundColor: 'rgba(0,0,0,0.5)'
                  }}
                >
                  <View style={styles.ModalInsideView}>
                    <Text
                      style={{
                        color: "black",
                        fontSize: 14,
                        fontWeight: "700",
                      }}
                    >
                      Hello{" "}
                    </Text>
                  </View>
                </View>
   </Modal>
</View>

const styles = {
  ModalInsideView: {
    justifyContent: "center",
    alignItems: "center",
    backgroundColor: "white",
    height: 245,
    width: "90%",
    borderRadius: 10,
    borderWidth: 1,
    borderColor: "#fff",
  },
};

```

![](../../.gitbook/assets/ying-mu-kuai-zhao-20201016-xia-wu-5.41.14.png)

